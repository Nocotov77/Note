# Note
### Обновление данных в базе
`
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
`

