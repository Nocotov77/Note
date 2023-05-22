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
### Раскрасить строки DataGrid в зависимости от параметра строки
```csharp
        private void materialsDataGridView_RowPrePaint(object sender, DataGridViewRowPrePaintEventArgs e)
        {
            try
            {
                foreach (DataGridViewRow row in materialsDataGridView.Rows)
                {
                    if (int.Parse(row.Cells[4].Value.ToString()) < int.Parse(row.Cells[5].Value.ToString()))
                        row.DefaultCellStyle.BackColor = Color.FromArgb(241, 146, 146);
                    if (int.Parse(row.Cells[4].Value.ToString()) >= int.Parse(row.Cells[5].Value.ToString()) * 3)
                        row.DefaultCellStyle.BackColor = Color.FromArgb(255, 186, 1);
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message);
            }
        }
```

### Сортировка через ComboBox
```csharp
        private void comboBoxCost_SelectedIndexChanged(object sender, EventArgs e)
        {
            if (comboBoxCost.Text == "По возрастанию")
            {
                comboBoxSortTitle.Text = ""; //Эти две строки очищают другие
                comboBoxQuantityStock.Text = "";
                materialsDataGridView.Sort(materialsDataGridView.Columns[3], ListSortDirection.Ascending);
            }

            if (comboBoxCost.Text == "По убыванию")
            {
                comboBoxSortTitle.Text = "";
                comboBoxQuantityStock.Text = "";
                materialsDataGridView.Sort(materialsDataGridView.Columns[3], ListSortDirection.Descending);
            }
        }
```

### Фильтрация через ComboBox
```csharp
        private void filterComboBox_SelectedIndexChanged(object sender, EventArgs e)
        {
            try
            {
                BindingSource source1 = new BindingSource();
                source1.DataSource = bigPack_26ip192MaterialsDataSet.materials;
                string find = Convert.ToString(searchTextBox.Text);
                if (filterComboBox.Text == "Любой тип материала")
                {
                    source1.Filter = " ";
                }
                else
                if (filterComboBox.Text == "Гранулы")
                {
                    source1.Filter = "[materialType] = 'Гранулы'";
                }
                else
                if (filterComboBox.Text == "Краски")
                {
                    source1.Filter = "[materialType] = 'Краски'";
                }
                else
                if (filterComboBox.Text == "Нитки")
                {
                    source1.Filter = "[materialType] = 'Нитки'";
                }
                materialsDataGridView.DataSource = source1;
                rows2 = materialsDataGridView.Rows.Count;
                labelCount.Text = Convert.ToString(rows2 + " из " + rows1);
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message);
            }
        }
```

### Поиск через TextBox, скомбинировано с фильтрацией через ComboBox
```csharp
        private void searchTextBox_TextChanged(object sender, EventArgs e)
        {
            try
            {
                BindingSource source1 = new BindingSource();
                source1.DataSource = bigPack_26ip192MaterialsDataSet.materials;
                string find = Convert.ToString(searchTextBox.Text);
                string filter = Convert.ToString(filterComboBox.Text);
                if (searchTextBox.Text == "")
                {
                    if (filterComboBox.Text == "Любой тип материала" ||
                    filterComboBox.Text == "Гранулы" ||
                    filterComboBox.Text == "Краски" ||
                    filterComboBox.Text == "Нитки")
                    {
                        if (filterComboBox.Text == "Любой тип материала")
                            source1.Filter = " ";
                        else
                            source1.Filter = "[materialType] = '" + filter + "'";
                    }
                    else
                    {
                        source1.Filter = " ";
                    }
                }
                else
                if (filterComboBox.Text == "Любой тип материала" ||
                    filterComboBox.Text == "Гранулы" ||
                    filterComboBox.Text == "Краски" ||
                    filterComboBox.Text == "Нитки")
                {
                    if (filterComboBox.Text == "Любой тип материала")
                    {
                        source1.Filter = "[title] LIKE '%" + find + "%'";
                    }
                    else
                    {
                        source1.Filter = "[materialType] = '" + filter + "' AND [title] LIKE '%" + find + "%'";
                    }
                }
                else
                {
                    MessageBox.Show("Выберите фильтр из выпадающего списка.");
                }
                materialsDataGridView.DataSource = source1;
                rows2 = materialsDataGridView.Rows.Count;
                labelCount.Text = Convert.ToString(rows2 + " из " + rows1);
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message);
            }
        }
```

### Открыть дополнительную форму
```csharp
        private void buttonAddMaterial_Click(object sender, EventArgs e)
        {
            Add Add = new Add(); // создаем объект класса Add
            Add.Show(); // делаем форму видимой
            this.Hide(); // скрываем текущую форму
        }
```

### Закрыть текущую дополнительную форму, раскрыть скрытую главную форму
```csharp
        private void Add_FormClosed(object sender, FormClosedEventArgs e)
        {
            // вызываем главную форму приложения, которая открыла текущую форму
            // главная форма всегда = 0
            Form MainForm = Application.OpenForms[0];
            MainForm.Show(); // делаем главную форму видимой
        }
```

### Закрыть форму
```csharp
        private void buttonBack_Click(object sender, EventArgs e)
        {
            this.Close(); //текущую форму закрываем
        }
```
