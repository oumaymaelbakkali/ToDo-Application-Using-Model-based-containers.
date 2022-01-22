# ToDo-Application-Using-Model-based-containers.
## Introduction 
As the firt version we will use QtDesigner to create MainWindow Application and Dialog Form , But for the Main window we will use tableview as a centralwidget.
## Creating Database
we will create an database qsllite ,and open it for creating an tasks table.
```cpp
   void ToDoApp::connectDatabase(){
    //cree une base de donnée qsllite
    db=QSqlDatabase::addDatabase("QSQLITE");
    db.setDatabaseName("C:\\Users\\uemf\\OneDrive\\ouma1.sqlite");
    //ouvrir  la base de donne
    if(!db.open())
        QMessageBox::critical(this,"Error","could not open the database");
    //cree la table des taches
    auto query = new QSqlQuery(db);
    QString create{"CREATE table IF  NOT EXISTS tasks (description VARCHAR(80),finished BOOLEAN,Tag VARCHAR(80),Date VARCHAR(80))"};
    if(!query->exec(create))
        QMessageBox::critical(this,"Error","could not create the database");
 }
```
## New task
When we clicked on New task action an Object D From Dialog class will created and then the user can enter task ,and if the replay accepted ,we will get their entries and we will put it in an table in the database with the function addEntry,but we have to check if the ckeckbox is checked and if the Due Date is toDay ;if the checkbox is not checked and the Due Dte is not to day we will create an  model  and an query of view and then associate model to query and to planing tableview,we will repeat the same operation but if the chekbox is checked wi will associate model to finished table view.  
```cpp
    void ToDoApp::on_actionNew_Task_triggered()
{
    Dialog D;
    QDateTime now = QDateTime::currentDateTime();
    auto replay=D.exec();

    if(replay== QDialog::Accepted && D.finish()==0 && D.CompareDate()==1){
        QStringList  tasks{D.Descri(),QString(D.finish()),D.Combo(),D.Date()};
        addEntry(D.Descri(),D.finish(),D.Combo(),D.Date());
                  //cree le model
                  auto model =new QSqlQueryModel;
                  //cree la request du view
                  auto query = QSqlQuery(db);
                  QString view{"SELECT * FROM tasks WHERE Date= '23/01/2022' "};
                   query.exec(view);
                  //associer le model a la requette
                  model->setQuery(query);
                  //assoier le model au view
                  ui->tableView->setModel(model);

    }else if(replay==QDialog::Accepted && D.finish()==0 && D.CompareDate()==0){
        QStringList  tasks2{D.Descri(),QString(D.finish()),D.Combo(),D.Date()};
         addEntry(D.Descri(),D.finish(),D.Combo(),D.Date());
              auto model =new QSqlQueryModel;
              //cree la request du view
              auto query = QSqlQuery(db);
              QString view{"SELECT * FROM tasks WHERE finished= FALSE AND Date!= '23/01/2022' "};
               query.exec(view);
              //associer le model a la requette
              model->setQuery(query);
              //assoier le model au view
              ui->tableView_2->setModel(model);
  }else if(replay== QDialog::Accepted && D.finish()==1 && D.CompareDate()==0){
        QStringList  tasks{D.Descri(),QString(D.finish()),D.Combo(),D.Date()};
         addEntry(D.Descri(),D.finish(),D.Combo(),D.Date());
              auto model =new QSqlQueryModel;
              //cree la request du view
              auto query = QSqlQuery(db);
              QString view{"SELECT * FROM tasks WHERE finished= TRUE"};
               query.exec(view);
              //associer le model a la requette
              model->setQuery(query);
              //assoier le model au view
              ui->tableView_3->setModel(model);
     }
     }
```
![image](https://user-images.githubusercontent.com/93142901/150658458-ff7a82ea-7e42-47c2-8964-187ed37de4cb.png)

## Planing task
```cpp
      void ToDoApp::on_actionPlaning_Task_triggered()
  {   ui->tableView_2->show();
    ui->tableView->hide();
    ui->tableView_3->hide();

   }
```
## Finished task 
```cpp 
      void ToDoApp::on_actionFinished_Task_triggered()
{
    ui->tableView_3->show();
    ui->tableView->hide();
    ui->tableView_2->hide();

}
```
## AboutQt
```cpp
       
  void ToDoApp::on_actionAbout_Qt_triggered()
  {
       QMessageBox::aboutQt(this,"AboutQT");
  }
```
##Quit
```cpp
       void ToDoApp::on_actionQuit_triggered()
   {
    this->close();
   }
  
```

## Conclusion 
