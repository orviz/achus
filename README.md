achus
=====

Accounting metric reporter.

# Definition
Make accounting reports from metrics gathered through connectors (GridEngine, ..)

# Connectors

A connector's duty consists in compute several metrics (CPU Time, Wallclock
Time, Efficiency, ..) by retrieving data from a given system's accounting.

## GridEngine connector (through SQL)

Instead of using either the accounting text file or the XML interface
offered by `qacct`, a SQL connector is the one offered to deal with 
GridEngine accounting data.

The connector itself does not store the data into a SQL database, it 
relies in an external script that does this job (e.g. cron basis). One 
solution is the one suggested [here](http://blog.adslweb.net/serendipity/article/270/Load-Grid-Engine-accounting-file-into-MySQL).


# Formatters
Formatters represent the accounting data (e.g. charts, text, ..)

## Charts
Using [pygal](http://pygal.org/).


# Renderers
Renderers stamp the content generated by the formatters.

## PDF
Generate PDF reports.


# Usage
```python
from reporter import Report

report = Report(
    "pdf",
    {
        "CPU usage per GROUP (in seconds)": {
            "connector": "ge",
            "metric": "cpu",
            "group_by": "group",
            "chart": "pie",
            "start_time": "2013-01-01 00:00",
            "end_time": "2013-02-01 00:00",
        },
        "Efficiency per GROUP": {
            "connector": "ge",
            "metric": "efficiency",
            "group_by": "group",
            "chart": "horizontal_bar",
        },
        "CPU usage per INFRASTRUCTURE (in seconds)": {
            "connector": "ge",
            "metric": "cpu",
            "group_by": "infrastructure",
            "chart": "pie",
        },
    },
    **{
        "filename": "/tmp/report.pdf",
    }
)
report.collect()
report.generate()
```
