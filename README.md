# How to display the total row count at the bottom, with active filtering in Flutter DataTable (SfDataGrid)?.

In this article, we will show you how to display the total row count at the bottom, with active filtering in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the necessary properties. You can fetch the total row count, even when filtering is applied, by using [DataGridSource.effectiveRows](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource/effectiveRows.html). By utilizing WidgetsBinding.instance.addPostFrameCallback in the initState, the row count is updated after the DataGrid completes its initial build. The [onFilterChanged](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/onFilterChanged.html) callback updates the row count in the UI whenever filtering is applied. The [footer](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/footer.html) of the SfDataGrid can display the total row count, and an additional row can be shown beneath the last row.

```dart
  int rowCount = 0;

  @override
  void initState() {
    super.initState();
    employees = getEmployeeData();
    employeeDataSource = EmployeeDataSource(employeeData: employees);
    // Update the rowCount after the data source is initialized.
    WidgetsBinding.instance.addPostFrameCallback((_) {
      setState(() {
        rowCount = employeeDataSource.effectiveRows.length;
      });
    });
  }

  void updateRowCount() {
    setState(() {
      rowCount = employeeDataSource.effectiveRows.length;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Syncfusion Flutter DataGrid'),
      ),
      body: SfDataGrid(
        source: employeeDataSource,
        allowFiltering: true,
        // Footer displaying the current row count.
        footer: Container(
            color: Colors.grey[400],
            child: Center(
                child: Text(
              'Total Rows count : $rowCount',
              style: const TextStyle(fontWeight: FontWeight.bold),
            ))),
        // Update row count when the filter is applied.
        onFilterChanged: (details) {
          updateRowCount();
        },
        columnWidthMode: ColumnWidthMode.fill,
        columns: <GridColumn>[
          GridColumn(
              columnName: 'id',
              label: Container(
                  padding: const EdgeInsets.all(16.0),
                  alignment: Alignment.center,
                  child: const Text(
                    'ID',
                  ))),
          GridColumn(
              columnName: 'name',
              label: Container(
                  padding: const EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: const Text('Name'))),
          GridColumn(
              columnName: 'designation',
              label: Container(
                  padding: const EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: const Text(
                    'Designation',
                    overflow: TextOverflow.ellipsis,
                  ))),
          GridColumn(
              columnName: 'salary',
              label: Container(
                  padding: const EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: const Text('Salary'))),
        ],
      ),
    );
  }
```

You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-display-the-total-row-count-at-the-bottom-in-Flutter-DataTable).