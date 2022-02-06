                    
                     Encryption   Interface
                       **Final Project**
                       
## Summary :
- [Overview ](#Overview )
-  [ App Structure](#App#Structure)
-   [ App Functions ](#App#Functions )
- [App Stylesheet](#App#Stylesheet)
-  [Conclusion](#Conclusion)
 
   ## Overview : 
   
   The goal of this project is to create a simple encryption app to encrypt and decrypt a text and text file, using what we've learned with Qt .
  We will use two encryption methods in this project Playfair and Hill .

## App Structure :

 The Main window of the program will be something like this :
 
![Screenshot 2022-02-06 175053](https://user-images.githubusercontent.com/78360286/152691905-60185cce-0b21-47c2-8666-722179bc6ea5.png)

We try to develop our app using only coding without using Qt Designer to practice more how the Qt platform works with its classes .

To get started we will have 4 file.h/cpp : 
 -> main file 
 -> encryptionapp ( to build our app )
 -> hill and playfair for encryption method 
 and we will divide the construction of our application into four parts :
#### Build Functionality Area : 
-> Create a lineEdit ( with a default value "hachimi" ) and a textEdit to insert and display the Key , plain Text and cipher Text :
```cpp
keyLnEdit  =  new  QLineEdit("hachimi",  this);
plainTextEdit  =  new  QTextEdit(this);
cipherTextEdit  =  new  QTextEdit(this);
 ```
 -> Create Lables : 
 ```cpp
  QLabel  *keyLbl  =  new  QLabel("Key:",  this);
  keyLbl->setAlignment(Qt::AlignRight|Qt::AlignVCenter);
 ```
  -> Create a QGridLayout : to manage of all the widgets and make the size and the position of a widget do not change if we resize the window.
  ```cpp
  QGridLayout  *settGrid  =  new  QGridLayout;
  settGrid->addWidget(keyLbl,  0,  0);
  settGrid->addWidget(keyLnEdit,  0,  1);
  settGrid->addWidget(plainLbl,  1,  0);
  settGrid->addWidget(plainTextEdit,  1,  1);
  settGrid->addWidget(cipherLbl,  2,  0);
  settGrid->addWidget(cipherTextEdit,  2,  1);
 ```
 #### Build Functionality  Area :
  -> we create a GroupBox "Functionality" that contain two Radio Button for hill and playfair encryption methods , and encryptor variable that take value of the Radio Button checked
  ```cpp
QGroupBox  *funcGroupBox  =  new   QGroupBox("Functionality",  this);
  playfairBtn  =  new  QRadioButton("Playfair",  this);
  hillBtn  =  new  QRadioButton("Hill",  this);
  playfairBtn->setChecked(true);
  encryptor  =  &playfair;
  QHBoxLayout  *funcHBox  =  new  QHBoxLayout;
  funcHBox->addWidget(playfairBtn);
  funcHBox->addWidget(hillBtn);
  funcGroupBox->setLayout(funcHBox);
 ```
#### Build Button Area :
we create tow pushButton "Encrtpt" and "Decrtpt " insede a GroupBox " Buttons "
```cpp
QHBoxLayout  *hbox3  =  new  QHBoxLayout;
  encryptBtn  =  new  QPushButton("Encrtpt",  this);
  decryptBtn  =  new  QPushButton("Decrtpt",  this);
  hbox3->addWidget(encryptBtn);
  hbox3->addWidget(decryptBtn);
  QGroupBox  *btnGroupBox  =  new  QGroupBox("Buttons",  this);
  btnGroupBox->setLayout(hbox3);
  QHBoxLayout  *btnHBox  =  new  QHBoxLayout;
  btnHBox->addWidget(btnGroupBox);
  vbox2->addLayout(btnHBox);
 ```
 #### Build Option Area :
we create tow CheckBox  to read from file ( to upload text file and encrypt it ) and to write to file ( to save encrpted text file ) insede a GroupBox "Options". 

```cpp
QVBoxLayout  *vbox3  =  new  QVBoxLayout;
  readFileChkBox  =  new  QCheckBox("Read from file",  this);
  writeFileChkBox  =  new  QCheckBox("Write to file",  this);
  vbox3->addWidget(readFileChkBox);
  vbox3->addWidget(writeFileChkBox);
  QGroupBox  *optionGroupBox  =  new  QGroupBox("  Options",  this);
  optionGroupBox->setLayout(vbox3);
  QHBoxLayout  *optionHBox  =  new  QHBoxLayout;
  optionHBox->addWidget(optionGroupBox);
  vbox2->addLayout(optionHBox);
 ```
 #### Build Display Area : 
 to display details about our key 
 ```cpp
dispLayout  =  new  QVBoxLayout;
  dispLabel  =  new  QLabel(this);
  dispLabel->setText(playfair.toString().c_str());
  dispLayout->addWidget(dispLabel);
  QGroupBox  *dispGroupBox  =  new  QGroupBox("Display detail",  this);
  dispGroupBox->setLayout(dispLayout);
  QHBoxLayout  *dispHBox  =  new  QHBoxLayout;
  dispHBox->addWidget(dispGroupBox);
  vbox2->addLayout(dispHBox);
 ```
  #### Build About Area :
  Create a label  just to give information about the App using some methods to text format and location .
  ```cpp
QLabel  *authorLbl  =  new  QLabel(
  "This  program  was  developed  by  Hachimi  ",
  this);
  authorLbl->setTextFormat(Qt::RichText);
  authorLbl->setGeometry(QRect(328,  240,  329,  27*4));
  authorLbl->setWordWrap(true);
  authorLbl->setAlignment(Qt::AlignTop);
  QVBoxLayout  *vbox4  =  new  QVBoxLayout;
  vbox4->addWidget(authorLbl);
  QGroupBox  *aboutGroupBox  =  new  QGroupBox("About",  this);
  aboutGroupBox->setLayout(vbox4);
  QHBoxLayout  *aboutHBox  =  new  QHBoxLayout;
  aboutHBox->addWidget(aboutGroupBox);
  vbox2->addLayout(aboutHBox);
 ```
After building all our areas :
 ![Screenshot 2022-02-06 205730](https://user-images.githubusercontent.com/78360286/152699038-1b54254e-4b12-4358-8812-f243f9d63305.png)
 
We finish the building of our app structure . Now we have to 
connect the objects between them and create functions to read from file , write to file , to encrpt and dcrypt ...

## App Functions :
#### Functions  :
First we create encryptable file.h that contains a structure to help us to manage our work : to take to key and use it in hill.cpp and playfair.cpp to call functions  from  hill/playfair.cpp when we call them .
encryptable file.h will be something like this :
```cpp
#ifndef  ENCRYPTABLE_H
#define  ENCRYPTABLE_H

#include  <string>
struct  Encryptable  {
  virtual  void  setKeyword(const  std::string&)  =  0;
  virtual  const  std::string  toString()  const  =  0;
  virtual  const  std::string  encrypt(const  std::string&)  const  =  0;
  virtual  const  std::string  decrypt(const  std::string&)  const  =  0;
  virtual  ~Encryptable()  {  }
};

#endif
 ```

Now we create functions inside encryptionapp.cpp to : 

 1. ReadFromFile :
 read from file function will be executed when we press the encrypt pushButton and the readFileChkBox is checked .
 -> we read the file using getOpenFileName() ,
 -> test if the file is empty then read the all the file by readAll() ;
 ```cpp
const  QString  ReadFromFile()
{
  const  QString  filename  =  QFileDialog::getOpenFileName(nullptr,
    "Choose file to read",  ".");
  if(filename.isEmpty())
  return  "";
  QFile  readFile(filename);
  return  readFile.readAll();
}
   ```
   ![Screenshot 2022-02-06 223701](https://user-images.githubusercontent.com/78360286/152702401-35a08bd2-29e2-4a5e-96aa-d8ded4dd19c6.png)

   
 2. WriteToFile :
After encrypt your text or our text file we have the option to save the encrypted message under a text file in our device .
we use getSaveFileName() to save new text file
```cpp
void  WriteToFile(const  QString&  content)
{
  const  QString  filename  =  QFileDialog::getSaveFileName(nullptr,
  "Create file to save result",  ".");
  if(filename.isNull())
  return;
  QFile  writeFile(filename);
  QTextStream  out(&writeFile);
  out  <<  content;
  
}
   ```
   ![Screenshot 2022-02-06 224915](https://user-images.githubusercontent.com/78360286/152702789-b8101f80-176c-4cbe-ac48-4bbc494add60.png)
   ![Screenshot 2022-02-06 224739](https://user-images.githubusercontent.com/78360286/152702798-5f230608-d028-49c7-bdde-8827a765fb74.png)

 3. onButtonClickEncrypt() :
we will create a function that will be executed when we press encrypt button : 
-> we test if read from file check box if is  "checked" so we have to display the text in plainTextEdit .
```cpp
void  MultiEncryption::onButtonClickEncrypt()
{
  QString  plainText;
  if(readFileChkBox->checkState()  ==  Qt::Checked)  {
  plainText  =  ReadFromFile();
  plainTextEdit->setText(plainText);
  }else  {
  plainText  =  plainTextEdit->toPlainText();
  }
   ```
 -> we take the key from keyLnEdit and we convert it from Qstring to string using toStdString()
 ```cpp
  const  QString  key  =  keyLnEdit->text();
  qDebug()  <<  "Input Key: "  +  key;
  qDebug()  <<  "Input plain text: "  +  plainText;
  encryptor->setKeyword(key.toStdString());
 ```
  -> we call encrypt function from hill.cpp or from playfair.cpp (depends on which radio box is checked) and we display cipher Text : 
  ( we will add later hill.cpp and playfair.cpp  with all fonctions )
```cpp
  const  QString  cipherText  =  encryptor->encrypt(
  plainText.toStdString()).c_str();
  qDebug()  <<  "Cipher text: "  +  cipherText;
  cipherTextEdit->setText(cipherText);
  dispLabel->setText(encryptor->toString().c_str());
  if(writeFileChkBox->checkState()  ==  Qt::Checked)
  WriteToFile(cipherText);
}
  ```
 and of course we have to check if write to file check box is checked to call WriteToFile function .
 
 4.  onButtonClickDecrypt () : 
this function is like onButtonClickEncrypt() , just we call now decrypt function : 
```cpp
void  MultiEncryption::onButtonClickDecrypt()
{
  QString  cipherText;
  if(readFileChkBox->checkState()  ==  Qt::Checked)  {
  cipherText  =  ReadFromFile();
  cipherTextEdit->setText(cipherText);
  }else  {
  cipherText  =  cipherTextEdit->toPlainText();
  }
  const  QString  key  =  keyLnEdit->text();
  qDebug()  <<  "Input Key: "  +  key;
  qDebug()  <<  "Input cipher text: "  +  cipherText;
  encryptor->setKeyword(key.toStdString());
  const  QString  plainText  =  encryptor->decrypt(
  cipherText.toStdString()).c_str();
  qDebug()  <<  "Plain text: "  +  plainText;
  plainTextEdit->setText(plainText);
  dispLabel->setText(encryptor->toString().c_str());
  if(writeFileChkBox->checkState()  ==  Qt::Checked)
  WriteToFile(plainText);
}
  ```
 5.  onRadioClickFunc : 
 we are using this function to detect which encryption methods we will use hill or playfair .
 -> we test witch radio box is checked , if hill method is checked we clear keyLnEdit ( key line edit )because we will use a standard key in hill encryption
  ```cpp
 void  MultiEncryption::onRadioClickFunc()
{
  if(sender()  ==  playfairBtn)  {
  encryptor  =  &playfair;
  keyLnEdit->setEnabled(true);
  encryptor->setKeyword(keyLnEdit->text().toStdString());
  dispLabel->setText(playfair.toString().c_str());
  }else  if(sender()  ==  hillBtn)  {
  encryptor  =  &hill;
  keyLnEdit->clear();
  keyLnEdit->setEnabled(false);
  dispLabel->setText(hill.toString().c_str());
  }
}
```
At the beginning we just create the structure base of our app then we create the functions that we need to use , now  we have to connect objects , we will use signal  to  slot .

Signals and slots are used for communication between objects , The concept is that GUI can send signals containing event information which can be received by other widgets using slots. 

 **connect  signal  to Slot**
 -> we connect encryptBtn pushButton with onButtonClickEncrypt() function :
   ```cpp
connect(encryptBtn,  SIGNAL(clicked(bool)), this,
  SLOT(onButtonClickEncrypt()));
```
-> decryptBtn pushButton with onButtonClickDecrypt()
 ```cpp
  connect(decryptBtn,  SIGNAL(clicked(bool)),  this,
  SLOT(onButtonClickDecrypt()));
```
-> playfairBtn Radio Button with  onRadioClickFunc()
```cpp
  connect(playfairBtn,  SIGNAL(clicked(bool)),  this,
  SLOT(onRadioClickFunc()));
```
-> hillBtn Radio Button with  onRadioClickFunc () 
```cpp
  connect(hillBtn,  SIGNAL(clicked(bool)),  this,
  SLOT(onRadioClickFunc()));
  ```
  
  All what we need now is hill and playfair file :  we will code hill and playfair algorithme based on what we see in cyber base classes 
  
  -> hill.cpp : 
  ```cpp
  using  namespace  std;

Hill::Hill()  :  rank(3),  keyMatrix(new  int*[3]),

  inverseKeyMatrix(new  int*[3])

{

  for(size_t  i  =  0;  i  <  rank;  i++)  {

  keyMatrix[i]  =  new  int[rank];
  inverseKeyMatrix[i]  =  new  int[rank];
  }
  int  keyArray[9]  =
  {17,  17,  5,
  21,  18,  21,
  2,  2,  19};
  int  inverseKeyArray[9]  =
  {4,  9,  15,
  15,  17,  6,
  24,  0,  17};
  for(size_t  row  =  0;  row  <  rank;  row++)  {
  for(size_t  col  =  0;  col  <  rank;  col++)  {
  keyMatrix[row][col]  =  keyArray[row*rank  +  col];
  inverseKeyMatrix[row][col]  =
  inverseKeyArray[row*rank  +  col];
  }
  }
}
Hill::~Hill()
{
  for(size_t  i  =  0;  i  <  rank;  i++)  {
  delete  []  keyMatrix[i];
  delete  []  inverseKeyMatrix[i];
  }
  delete  []  keyMatrix;

  delete  []  inverseKeyMatrix;

}

std::ostream&  operator<<(std::ostream&  os,
  const  Hill&  hill)
{
  for(size_t  row  =  0;  row  <  hill.rank;  row++)  {

  for(size_t  col  =  0;  col  <  hill.rank;  col++)  {

  os  <<  hill.keyMatrix[row][col]  <<  '\t';

  }

  os  <<  endl;

  }

  return  os;

}
namespace  {

void  SquareMatrixMulToColumnVector(

  size_t  rank,

  const  int  *  const  *  const  squareMatrix,  //  squareMatrix[rank][rank]

  const  int  *  const  rightMatrix,  //  rightMatrix[rank][1]

  int  *  const  resultMatrix)  //  resultMatrix[rank][1]
{
  for(size_t  row  =  0;  row  <  rank;  row++)  {

  int  sum  =  0;

  for(size_t  col  =  0;  col  <  rank;  col++)

  sum  +=  squareMatrix[row][col]  *  rightMatrix[col];

  resultMatrix[row]  =  sum;

  }

}

}
const  std::string  Hill::encrypt(

  const  std::string&  plainText)  const

{

  if(plainText.empty())  return  "";

  string  cipherText;

  cipherText.reserve(plainText.size());

  string::const_iterator  pIter  =  plainText.begin(),

  pIterEnd  =  plainText.end();

  auto  cipherBInserter  =  back_inserter(cipherText);
  int  *plainAlphaGroup  =  new  int[rank];

  int  *cipherAlphaGroup  =  new  int[rank];
  while(pIter  !=  pIterEnd)  {

  //  collect  alphas

  string::const_iterator  tmpIter  =  pIter;

  size_t  cnt  =  0;

  while(tmpIter  !=  pIterEnd  &&  cnt  <  rank)  
  if(isalpha(*tmpIter))  {
  plainAlphaGroup[cnt]  =  toupper(*tmpIter)  -  'A';
  cnt++;
  }
  tmpIter++;
 }
  //  if  not  have  enough  alpha  to  encrypt,  we  must  add  filled  alpha
  while(cnt  <  rank)  {
  plainAlphaGroup[cnt]  =  'X'  -  'A';
 cnt++;
  }
  stringstream  ss;
  for(size_t  i  =  0;  i  <  rank;  i++)
  ss  <<  plainAlphaGroup[i]  <<  " ";
  //  encrypt  alpha
  SquareMatrixMulToColumnVector(rank,  keyMatrix,
  plainAlphaGroup,
  cipherAlphaGroup);
  //  insert  encrypted  characters  to  cipher  text
  for(cnt  =  0;  pIter  !=  pIterEnd  &&  cnt  <  rank;  pIter++)  {
  if(isalpha(*pIter))  {
  *cipherBInserter++  =  cipherAlphaGroup[cnt]%26  +  'A';
  cnt++;
 }else  {
  *cipherBInserter++  =  *pIter;
  }
  }
  //  insert  the  filled  alpha
  while(cnt  <  rank)  {
  *cipherBInserter++  =  cipherAlphaGroup[cnt]%26  +  'A';
  cnt++;
  }
  }
  delete  plainAlphaGroup;
  delete  cipherAlphaGroup;
  return  cipherText;
}
const  std::string  Hill::decrypt(
  const  std::string&  cipherText)  const
{
  if(cipherText.empty())  return  "";
  string  plainText;
  plainText.reserve(cipherText.size());
  string::const_iterator  cipherIter  =  cipherText.begin(),
  cipherIterEnd  =  cipherText.end();
  auto  plainBInserter  =  back_inserter(plainText);
  int  *cipherAlphaGroup  =  new  int[rank];
  int  *plainAlphaGroup  =  new  int[rank];
  while(cipherIter  !=  cipherIterEnd)  {
  //  collect  alphas
  string::const_iterator  tmpIter  =  cipherIter;
  size_t  cnt  =  0;
  while(tmpIter  !=  cipherIterEnd  &&  cnt  <  rank)  {
  if(isalpha(*tmpIter))  {
  cipherAlphaGroup[cnt]  =  toupper(*tmpIter)  -  'A';
  cnt++;
  }
  tmpIter++;
  }
  //  decrypt  alpha
  SquareMatrixMulToColumnVector(rank,  inverseKeyMatrix,
  cipherAlphaGroup,
  plainAlphaGroup);
  //  write  decrypted  characters  to  plain  text
  for(cnt  =  0;  cipherIter  !=  cipherIterEnd  &&  cnt  <  rank;
  cipherIter++)  {
  if(isalpha(*cipherIter))  {
  *plainBInserter++  =  tolower(
  plainAlphaGroup[cnt]%26  +  'A');
  cnt++;
  }else  {
  *plainBInserter++  =  *cipherIter;
             }
     }
  }
  delete  plainAlphaGroup;
  delete  cipherAlphaGroup;
  return  plainText;
}
  ```
  -> playfair.cpp : 
```cpp 
Playfair::Playfair(const  std::string&  keyword)
{
  setKeyword(keyword);
}
bool  Playfair::fillMatrixAndIsFull(char  ch,  std::size_t&  row,

  std::size_t&  col)

{

  //  we  don't  insert  'J',  wherever  replacing  'J'  with  'I'

  if(ch  ==  'J')

  return  false;

  

  alphaMatrix[row][col]  =  ch;

  alphaPosMap.insert(make_pair(ch,

  make_pair(row,  col)));

  if(++col  ==  RANK)  {

  col  =  0;

  if(++row  ==  RANK)
  return  true;
  }

  return  false;

}
void  Playfair::setKeyword(const  std::string&  keyword)

{

  //  filter  nonalphabetic  characters

  std::string  key;

  key.reserve(keyword.size());

  auto  keyBInserter  =  back_inserter(key);

  copy_if(keyword.begin(),  keyword.end(),  keyBInserter,

  ptr_fun<int,  int>(isalpha));

  

  set<char>  alphaSet;

  size_t  i  =  0,  j  =  0;

  

  alphaPosMap.clear();
  //  firstly  insert  keyword  to  alpha  matrix

  for(string::const_iterator  keyIter  =  key.begin();

  keyIter  !=  key.end();  keyIter++)  {

  char  ch  =  toupper(*keyIter);

  if(alphaSet.find(ch)  ==  alphaSet.end())  {

  if(fillMatrixAndIsFull(ch,  i,  j))

  break;

  alphaSet.insert(ch);
  }
  }
  //  secondly  fill  the  alpha  matrix  with  remaining  alphas

  for(char  ch  =  'A';  ch  !=  'Z'  +  1;  ch++)  {

  if(alphaSet.find(ch)  ==  alphaSet.end())  {

  if(fillMatrixAndIsFull(ch,  i,  j))

  break;

  alphaSet.insert(ch);

  }

  }
  //  last,  to  speed  up  the  encryption,  we  fill  the  edge  of  alpha  matrix

  for(size_t  k  =  0;  k  <  RANK;  k++)

  alphaMatrix[RANK][k]  =  alphaMatrix[0][k];

  for(size_t  k  =  0;  k  <  RANK;  k++)

  alphaMatrix[k][RANK]  =  alphaMatrix[k][0];

  

  alphaMatrix[RANK][RANK]  =  ' ';

}
const  std::pair<size_t,  size_t>  Playfair::getRowAndCol(char  ch)  const

{

  if('J'  ==  ch)  ch  =  'I';

 
  map<char,  pair<size_t,  size_t>  >::const_iterator

  mapIter  =  alphaPosMap.find(ch);

  if(mapIter  ==  alphaPosMap.end())

  return  make_pair(RANK,  RANK);

  return  mapIter->second;

}
void  Playfair::doEncrypt(char&  ch1,  char&  ch2)  const

{

  const  pair<size_t,  size_t>  pair1  =  getRowAndCol(ch1),

  pair2  =  getRowAndCol(ch2);

  if(pair1.first  ==  pair2.first)  {//  ch1  &  ch2  in  the  same  row

  ch1  =  alphaMatrix[pair1.first][pair1.second  +  1];

  ch2  =  alphaMatrix[pair2.first][pair2.second  +  1];

  }else  if(pair1.second  ==  pair2.second)  {//  in  the  same  col

  ch1  =  alphaMatrix[pair1.first  +  1][pair1.second];

  ch2  =  alphaMatrix[pair2.first  +  1][pair2.second];

  }else  {  //  not  in  the  same  row  or  col

  ch1  =  alphaMatrix[pair1.first][pair2.second];

  ch2  =  alphaMatrix[pair2.first][pair1.second];

  }

}
void  Playfair::doDecrypt(char&  ch1,  char&  ch2)  const

{

  const  pair<size_t,  size_t>  pair1  =  getRowAndCol(ch1),

  pair2  =  getRowAndCol(ch2);

  if(pair1.first  ==  pair2.first)  {//  ch1  &  ch2  in  the  same  row

  ch1  =  alphaMatrix[pair1.first][(RANK+pair1.second-1)%RANK];

  ch2  =  alphaMatrix[pair2.first][(RANK+pair2.second-1)%RANK];

  }else  if(pair1.second  ==  pair2.second)  {//  in  the  same  col

  ch1  =  alphaMatrix[(RANK+pair1.first-1)%RANK][pair1.second];

  ch2  =  alphaMatrix[(RANK+pair2.first-1)%RANK][pair2.second];

  }else  {  //  not  in  the  same  row  or  col

  ch1  =  alphaMatrix[pair1.first][pair2.second];

  ch2  =  alphaMatrix[pair2.first][pair1.second];

  }

}
const  std::string  Playfair::encrypt(

  const  std::string&  plainText)  const
{
  if(plainText.empty())  return  "";

  string  cipherText;

  cipherText.reserve(plainText.size()  *  2)
  
  string::const_iterator  pIter  =  plainText.begin(),

  pIterEnd  =  plainText.end();

  auto  cipherBIter  =  back_inserter(cipherText);

  while(pIter  !=  pIterEnd)  {

  char  ch1  =  toupper(*pIter++);

  char  ch2;

  bool  hasInsertAlpha  =  false;

  //  skip  nonalphabetic  characters

  if(!isalpha(ch1))  {

  *cipherBIter++  =  ch1;

  continue;

  }

  string::const_iterator  tmpIter  =  pIter;

  while(tmpIter  !=  pIterEnd)  {

  if(isalpha(*tmpIter))  {

  ch2  =  toupper(*tmpIter);

  //  nearby  alpha  is  same,  so  we  need  to  insert  a  filled

  //  alpha  in  the  middle  of  then

  if(ch1  ==  ch2)

  ch2  =  (ch1  ==  'K')  ?  'Z'  :  'K'  ;

  break;

  }

  tmpIter++;

  }

  //  less  two  alpha,  so  we  need  append  a  filled  alpha
  if(tmpIter  ==  pIterEnd)  {
  hasInsertAlpha  =  true;
  ch2  =  (ch1  ==  'K')  ?  'Z'  :  'K'  ;
  }
  doEncrypt(ch1,  ch2);
  *cipherBIter++  =  ch1;
  while(pIter  !=  pIterEnd)  {
  if(isalpha(*pIter))  {
  *cipherBIter++  =  ch2;
  pIter++;
  break;
  }
  *cipherBIter++  =  *pIter++;
  }
  if(hasInsertAlpha)

  *ciherBIter++  =  ch2;
  }
  return  cipherText;
}
const  std::string  Playfair::decrypt(
  const  std::string&  cipherText)  const
{
  if(cipherText.empty())  return  "";
  string  plainText;
  plainText.reserve(plainText.size());
  string::const_iterator  cipherIter  =  cipherText.begin(),
  cipherIterEnd  =  cipherText.end();
  auto  plainBInserter  =  back_inserter(plainText)
  
  while(cipherIter  !=  cipherIterEnd)  {

  char  ch1  =  toupper(*cipherIter++);

  char  ch2  =  'K';
  bool  hasFillAlpha  =  false

  //  skip  nonalphabetic  characters

  if(!isalpha(ch1))  {
  *plainBInserter++  =  ch1;
  continue;
  }
  string::const_iterator  tmpIter  =  cipherIter;
  while(tmpIter  !=  cipherIterEnd)  {

  if(isalpha(*tmpIter))  {
  ch2  =  toupper(*tmpIter);
  break;
  }
  tmpIter++;
  }
  if(tmpIter  ==  cipherIterEnd)

  hasFillAlpha  =  true;
  doDecrypt(ch1,  ch2);
  *plainBInserter++  =  ch1;
  while(cipherIter  !=  cipherIterEnd)  {
  if(isalpha(*cipherIter))  {
  *plainBInserter++  =  ch2;
  cipherIter++;
  break;
  }
  *plainBInserter++  =  *cipherIter++;
  }
  if(hasFillAlpha)
  *plainBInserter++  =  ch2;
  }
  return  plainText;
}
std::ostream&  operator<<(std::ostream&  os,

  const  Playfair&  p)

{

  for(size_t  row  =  0;  row  <  Playfair::RANK+1;  row++)  {

  for(size_t  col  =  0;  col  <  Playfair::RANK+1;  col++)  {

  char  ch  =  p.alphaMatrix[row][col];

  if(ch  ==  'I')
  os  <<  "I/J ";
  else
  os  <<  ch  <<  "   ";
  }
  os  <<  endl;
  if(Playfair::RANK-1  ==  row)
  os  <<  endl;
  }
  return  os;
}
```
Now our App is ready .

## App stylesheet :
After finishing our app : 
![Screenshot 2022-02-06 205730](https://user-images.githubusercontent.com/78360286/152703614-1ded263e-6e65-4ace-a9da-3bcc39c8f206.png)

We will try to style and designour app using css file :
-> First we create a resource "mystyle" file in our project and we add a style.css file .
-> in the main function  : to read style.css :
```cpp
int  main(int  argc,  char  *argv[])
{
  QApplication  a(argc,  argv);  
  QFile  file(":/mystyle/style.css");
  file.open(QFile::  ReadOnly);
  QString  StyleSheet  =  QLatin1String(file.readAll());
  encryptionapp w;
  w.setStyleSheet(StyleSheet);
  w.setWindowTitle("EncryptionApp");
  w.resize(685,  425);
  w.show();
  return  a.exec();
}
```
-> in the style.css  :
we give the PushButton a specific color with a specific background and border color, size, font and style ... 
```cpp 
QPushButton
{
  color:  #b1b1b1;
  background-color:  QLinearGradient(  x1:  0,  y1:  0,  x2:  0,  y2:  1,  stop:  0  #06193a,  stop:  0.1  #06193a,  stop:  0.5  #06193a,  stop:  0.9  #06193a,  stop:  1  #06193a);
  border-width:  1px;
  border-color:  #f3520e;
  border-style:  outset  ;
  border-radius:  6;
  padding:  3px;
  font-size:  12px;
  padding-left:  5px;
  padding-right:  5px;
  font:  9pt  "Arial";
}
```
we do the same for all objects : 
```cpp QGroupBox  {
  background-color:  #848599  ;
  color:  #9e3305  ;
  subcontrol-position:  top  left;  /*  position  at  the  top  left*/
  border:  1px  solid  #f3520e;
  border-radius:  9px;
  font:  9pt  "Arial";
}
QCheckBox{
  color:  #06193a  ;
  border-style:  outset  ;
  border:  2px  solid  #f3520e  ;
  icon-size:  15px  ;
  font:  9pt  "Arial";
}
QLabel
{
  color:  #06193a  ;
  border-radius:  10px;
  font:  9pt  "Arial";
}
QTextEdit{
  color:  #ffffff  ;
  border-width:  1px;
  background-color:  QLinearGradient(  x1:  0,  y1:  0,  x2:  0,  y2:  1,  stop:  0  #06193a,  stop:  0.1  #06193a,  stop:  0.5  #06193a,  stop:  0.9  #06193a,  stop:  1  #06193a);
  border-color:  #f3520e;
  border-style:  outset  ;
  border-radius:  6;
  padding:  3px;
  font-size:  12px;
  pading-left:  5px;
  padding-right:  5px;
  font:  9pt  "Arial";
}
QLineEdit{
  color:  #ffffff  ;
  border-width:  1px;
  background-color:  QLinearGradient(  x1:  0,  y1:  0,  x2:  0,  y2:  1,  stop:  0  #06193a,  stop:  0.1  #06193a,  stop:  0.5  #06193a,  stop:  0.9  #06193a,  stop:  1  #06193a);
  border-color:  #f3520e;
  borde-style:  outset  ;
  border-radius:  6;
  padding:  3px;
  font-size:  12px;
  padding-left:  5px;
  padding-right:  5px;
  font:  9pt  "Arial" ;
}
QRadioButton  {
color:  #06193a  ;
border-width:  1px;
padding:  3px;
font-size:  12px;
padding-left:  5px;
  padding-right:  5px;
font:  9pt  "Arial";
}
```
the result will be something like this : 

![Screenshot 2022-02-06 175053](https://user-images.githubusercontent.com/78360286/152704253-a3572b5d-0875-4529-902a-71360f541c3a.png)

## Conclusion

In this Project we learned to interactive with Qt using all what what we get in our class . it was a really good experience and I am glad to be able to complete this project . 

Thank You .
