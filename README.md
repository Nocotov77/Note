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

### Пример авторизации
```csharp
        private void loginButton_Click(object sender, EventArgs e)
        {
            int loginFindIndex = accountBindingSource.Find("login", loginTextBox.Text);
            int passFindIndex = accountBindingSource.Find("pass", passTextBox.Text);
            
            if (loginFindIndex == passFindIndex && loginFindIndex > -1 && passFindIndex > -1)
            {
                accountBindingSource.Position = loginFindIndex;
                int acessLevel = Convert.ToInt32(((DataRowView)accountBindingSource.Current).Row["acessLevel"]);
                if (acessLevel == 1)
                {
                    loginTextBox.Text = "";
                    passTextBox.Text = "" ;
                    MainForm mainForm = new MainForm();
                    mainForm.Show();
                    this.Hide();
                }
                else
                {
                    MessageBox.Show("Гостевой режим.");
                }
            }
            else
            {
                MessageBox.Show("Неверный логин/пароль.");
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

### Пример руководства пользователя
```
«Руководство оператора»
Программного модуля «Отделение колледжа».
1.	Назначение программы.
Данная программа предназначена для Отдела Кадров Колледжа. Она позволяет редактировать личные дела сотрудников (преподавателей) и изучать их личные дела, также диспетчер может редактировать расписание занятий. 
2.	Условия выполнения программы.
Минимальная конфигурация:
	• тип процессора: Celeron и выше;
	• объем оперативного запоминающего устройства: 2 Гб и более;
	• объем свободного места на жестком диске: 1 Гб.
Рекомендуемая конфигурация:
	• тип процессора: Intel Core i3;
	• объем оперативного запоминающего устройства: 4 Гб и более;
	• объем свободного места на жестком диске: 1,5 Гб.

3.	Выполнение программы.
Рис 1. Контекстное меню.
Раздел файл позволяет открывать карточки сотрудников, созданные в формате .txt, а также закрыть программу.
Кнопка «О программе…» открывает раздел о программе.
Рис 2. Карточка сотрудника
В карточку сотрудника отображаются данные, введённые в форму ранее.
Рис 3. Окно базы данных
Это окно разделено на две панели. Верхняя панель отображает таблицы базы данных и результат команд, а в нижнюю панель вводятся SQL-команды.
4.	Сообщения оператору.
Сообщения не предусмотрен.
```

# КАПТЧА
Создайте проект Windows Form и добавьте три компонента на форму:

pictureBox1 — будет отвечать за прорисовку капчи.
Свойство: Size = 169; 63;

textBox1 -нужен для ввода пользователем ответа
button1 — кнопка для генерации новой капчи.
Свойство: Text = Обновить;

button2 — кнопка проверки введенных данных
Свойство: Text = OK; 

После добавления компонентов ваша форма примет приведенный ниже вид:
![Форма с каптчей]()

Перейдите в код формы и добавьте следующий метод:
```csharp
private Bitmap CreateImage(int Width, int Height)
{
Random rnd = new Random();

//Создадим изображение
Bitmap result = new Bitmap(Width, Height);

//Вычислим позицию текста
int Xpos = 10;
int Ypos = 10;

//Добавим различные цвета ддя текста
Brush[] colors = {
Brushes.Black,
Brushes.Red,
Brushes.RoyalBlue,
Brushes.Green,
Brushes.Yellow,
Brushes.White,
Brushes.Tomato,
Brushes.Sienna,
Brushes.Pink };

//Добавим различные цвета линий
Pen[] colorpens = {
Pens.Black,
Pens.Red,
Pens.RoyalBlue,
Pens.Green,
Pens.Yellow,
Pens.White,
Pens.Tomato,
Pens.Sienna,
Pens.Pink };

//Делаем случайный стиль текста
FontStyle[] fontstyle = {
FontStyle.Bold,
FontStyle.Italic,
FontStyle.Regular,
FontStyle.Strikeout,
FontStyle.Underline};

//Добавим различные углы поворота текста
Int16[] rotate = {1,-1,2,-2,3,-3,4,-4,5,-5,6,-6};

//Укажем где рисовать
Graphics g = Graphics.FromImage((Image)result);

//Пусть фон картинки будет серым
g.Clear(Color.Gray);

//Делаем случайный угол поворота текста
g.RotateTransform(rnd.Next(rotate.Length));

//Генерируем текст
text = String.Empty;
string ALF = "


7890QWERTYUIOPASDFGHJKLZXCVBNM";
for (int i = 0; i < 5; ++i)
text += ALF[rnd.Next(ALF.Length)];

//Нарисуем сгенирируемый текст
g.DrawString(text,
new Font("Arial", 25, fontstyle[rnd.Next(fontstyle.Length)]),
colors[rnd.Next(colors.Length)],
new PointF(Xpos, Ypos));

//Добавим немного помех
//Линии из углов
g.DrawLine(colorpens[rnd.Next(colorpens.Length)],
new Point(0, 0),
new Point(Width - 1, Height - 1));
g.DrawLine(colorpens[rnd.Next(colorpens.Length)],
new Point(0, Height - 1),
new Point(Width - 1, 0));

//Белые точки
for (int i = 0; i < Width; ++i)
for (int j = 0; j < Height; ++j)
if (rnd.Next() % 20 == 0)
result.SetPixel(i, j, Color.White);

return result;
}
```

В событие Click кнопки button1 добавим метод для генерации новой капчи:
```csharp
private void button1_Click(object sender, EventArgs e)
{
pictureBox1.Image = this.CreateImage(pictureBox1.Width, pictureBox1.Height);
}
```

В событие Form1_Load, запуск которого происходит при загрузке проекта, так же добавим генерацию капчи.
```csharp
private void Form1_Load(object sender, EventArgs e)
{
pictureBox1.Image = this.CreateImage(pictureBox1.Width, pictureBox1.Height);
}
```
