---
layout: post
title: UiPath PivotTable Activity
comments: true
description: Custom UiPath Activity in order to input a DataTable and transform this table into a PivotTable
published: true
categories:
- csharp
tags:
- UiPath
- csharp
- PivotTable
- DataTable
- Activities
- Custom Activity
- RPA
---

It's been a little while since my last post and I thought, you know, it's time to post something. So in the last months I started to using a RPA tool called UiPath, very curious and interesting, but the most interesting thing is the extensibility that you can do with nuget packages.

Following the official [documentation](https://activities.uipath.com/docs/creating-a-custom-activity){:target="_blank"} how to create a custom activity with UiPath I created one. The activity receive a DataTable and returns the data on a Pivot DataTable.

So the idea is get this:

<div class="row previews" align="center">
		<img class="img-responsive" alt="DataTable to Pivot DataTable" src="/images/DataTableToPivot.png">
</div>

So I got the Pivot DataTable function that is around the stackoverflow and I created the Activity, here is the code: 

{% highlight csharp %} 
using System;
using System.Activities;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace PivotDataTableActivity
{
    public class PivotTable : CodeActivity
    {
        [Category("Input")]
        [RequiredArgument]
        public InArgument<DataTable> InputDataTable { get; set; }

        [Category("Input")]
        [RequiredArgument]
        public InArgument<DataColumn> PivotColumn { get; set; }

        [Category("Input")]
        [RequiredArgument]
        public InArgument<DataColumn> PivotValue { get; set; }

        [Category("Output")]
        public OutArgument<DataTable> OutPutDataTable { get; set; }

        protected override void Execute(CodeActivityContext context)
        {
            DataTable dataTable = InputDataTable.Get(context);
            DataColumn dataColumn = PivotColumn.Get(context);
            DataColumn dataValue = PivotValue.Get(context);

            DataTable result = Pivot(dataTable, dataColumn, dataValue);
            OutPutDataTable.Set(context, result);
        }

        static DataTable Pivot(DataTable dt, DataColumn pivotColumn, DataColumn pivotValue)
        {
            // find primary key columns 
            //(i.e. everything but pivot column and pivot value)
            DataTable temp = dt.Copy();
            temp.Columns.Remove(pivotColumn.ColumnName);
            temp.Columns.Remove(pivotValue.ColumnName);
            string[] pkColumnNames = temp.Columns.Cast<DataColumn>()
                .Select(c => c.ColumnName)
                .ToArray();

            // prep results table
            DataTable result = temp.DefaultView.ToTable(true, pkColumnNames).Copy();
            result.PrimaryKey = result.Columns.Cast<DataColumn>().ToArray();
            dt.AsEnumerable()
                .Select(r => r[pivotColumn.ColumnName].ToString())
                .Distinct().ToList()
                .ForEach(c => result.Columns.Add(c, pivotColumn.DataType));

            // load it
            foreach (DataRow row in dt.Rows)
            {
                // find row to update
                DataRow aggRow = result.Rows.Find(
                    pkColumnNames
                        .Select(c => row[c])
                        .ToArray());
                // the aggregate used here is LATEST 
                // adjust the next line if you want (SUM, MAX, etc...)
                aggRow[row[pivotColumn.ColumnName].ToString()] = row[pivotValue.ColumnName];
            }

            return result;
        }
    }
}
{% endhighlight %}

Compile, package the .dll with [Nuget Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer/releases) and add the nuget package activity to the UiPath and will appears in the activities section. Then drag and drop the activity, send the DataTable input with the column and value that you want to pivot and done. 

<div class="row previews" align="center">
		<img class="img-responsive" alt="UiPath Pivot DataTable" src="/images/PivotCustomActivity.png">
</div>

### Files:

* [UiPath Project Sample](/media/PivotDataTable.zip)
* [Nuget Package Pivot DataTable Activity](/media/ActivitiesPivotTable.1.0.0.nupkg)

So far so good, right!

references

* [https://stackoverflow.com/questions/12866685/dynamic-pivot-using-c-sharp-linq](https://stackoverflow.com/questions/12866685/dynamic-pivot-using-c-sharp-linq){:target="_blank"}
* [https://techbrij.com/pivot-c-array-datatable-convert-column-to-row-linq](https://techbrij.com/pivot-c-array-datatable-convert-column-to-row-linq){:target="_blank"}


