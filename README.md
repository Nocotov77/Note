# Note
### Обновление данных в базе
```csharp
try
  {
    materialsTableAdapter.Update(bigPack_26ip192MaterialsDataSet.materials); 
    // Вместо materialsTableAdapter нужно подставить tableAdapter той БД, с которой необходимо работать
    // А вместо bigPack_26ip192MaterialsDataSet.materials необходимо подставить DataSet той БД, с которой необходимо работать
    // tableAdapter и DataSet можно получить в обработчике Load формы
    MessageBox.Show("База данных обновлена.");
  }
catch (Exception ex)
  {
    MessageBox.Show(ex.Message);
  }
```

### Удалить строчки с DataGrid
```csharp
try
{
  if (materialsDataGridView.SelectedRows.Count == 1)
  {
    int selectedIndex = materialsDataGridView.CurrentRow.Index;
    MessageBox.Show("Удалено " + materialsDataGridView.SelectedRows.Count + " строк. Обновите базу данных!");
    materialsDataGridView.Rows.Remove(materialsDataGridView.Rows[selectedIndex]);
  }
  else if (materialsDataGridView.SelectedRows.Count > 1)
  {
    MessageBox.Show("Удалено " + materialsDataGridView.SelectedRows.Count + " строк. Обновите базу данных!");
    while (materialsDataGridView.SelectedRows.Count > 0)
    {
      int selectedIndex = materialsDataGridView.CurrentRow.Index;
      materialsDataGridView.Rows.Remove(materialsDataGridView.Rows[selectedIndex]);
    }
  }
}
catch (Exception ex)
{
  MessageBox.Show(ex.Message);
}
```

### Получение данных из строки DataGrid
```csharp
int quantityMin = int.Parse(materialsDataGridView.CurrentRow.Cells[5].Value.ToString());
int quantityStock = int.Parse(materialsDataGridView.CurrentRow.Cells[4].Value.ToString());
int quantityPack = int.Parse(materialsDataGridView.CurrentRow.Cells[6].Value.ToString());
double cost = double.Parse(materialsDataGridView.CurrentRow.Cells[3].Value.ToString());
```
