# Specification-based on Use Cases (uc_3_SoftRobot) 


DataEntities: 

>
>DataEntity e_backOfficeOperator is a Master […].  
>
>DataEntity e_organizationalEntity is a Master […].  
>
>DataEntity e_document is a Document […].  

Actors: 

>Actor a_backOfficeOperator (Operator) is a User who Process documents. 
>
>Actor a_OrganizationalEntity (Customer) is a User, who creates documents. 
>
>Actor, a_Robot (Robot), is the robot responsible for robotic process automation. 
>

Use Cases: 

>UseCase uc_3_SoftRobot is a EntitiesManage with e_document, actor_a_Robot. 


# Specification-based on Use Cases Scenarios

Use Cases Scenarios: 

UseCase uc_3_SoftRobot 
[…] 
>s0. Scenario MainScenario (Main): 
>
>s1. Robot: Get from a specific location folder the list of digitalized documents (in a “pdf” format), with file names outside the standard format. 
>
>s2. Robot: Read and convert each document from “pdf” into “txt” format, using OCR technology with a scale of zero. 
>
>s3. Robot: Browses the list of Documents and opens each one of them in “txt” format. 
>
>s4. Robot: For each document, extract and insert in an excel file the following text fragments: document type, organizational entity (responsible for document  >creation), process number and creation year.  
>
>s5. Robot: In addiction, insert into the excel file the path where the document is located. 
>
>s6. Robot: Get the document filename in the correct standard format from the Excel file and change the filename of each one. 
>
>s7. Robot: If the document filename is in the correct format then upload the file into the data store of the SICMAR application and move it to the Processed Document >Folder.  
>
>s8. Robot: In case the document file has an incorrect format filename then move it to the Failed Document Folder. 
>
>s9. Robot: Send an email message to the IT Manager specifying how many and which documents have already been successfully inserted into the system and how many and >which have failed. 

# Specification based on Pseudocode

Soft Robot SICMAR: 

//1st: declaration of variables 
>
> pdfPath = Environment.CurrentDirectory  
>
>  pdfFiles = Directory.GetFiles(pdfPath,"*.pdf") 
>
>  totalNumberOfPdfFiles = 0   
>
>  numberOfFinishedPdfFiles = 0  
>
>  numberOfFailedPdfFiles = 0  
>
>  extratedText, strtype, strNumber, strEntity, strCreationYear, strOC, newPdfFileName, oldPdfFileName 
>
//2nd: read and convert document file from “pdf” //into “txt” format 
>
>begin 
>
>  FOR each pdfFile In pdfFiles 
>
>   […] 
>
>    READ pdfFile with OmniPage OCR (SCALE(0)) 
>
>    WRITE extractedText  	  	 
>
>  END IF   
>
//3rd: replace wrong format information in the //document file 
>
>ExtratecdText = strText.Replace("Nº:","Number:") 
>
>ExtratecdText = strText.Replace("From.","From:") 
>
>ExtratecdText = strText.Replace("'","") 
>
//4th: extract information from the document file //using regular expressions 
>
>IF String.IsNullOrEmpty(strCreationYear) 
>
>  StrCreationYear = System.Text.RegularExpressions.Regex.Match(strText,"(?i)(?<=Data:\s)(\d{2}.\d{2}.\d{4})").Value 
>
>END IF 
>
//5th: write extracted information from the //document file in the excel file 
>
>   WRITE strType  	       
>
>    WRITE strEntity  	       
>
>    WRITE strProcessNumber      
>
>    WRITE strCreationYear  	 
>
//6th: read new document filename in the excel  //file  
>
>    READ 	newPdfFileName 
>
>  IF cell.Length < 17 OR cell.Length > 19 OR cell.Contains("/") OR String.IsNullOrEmpty(cell) 
>
//7th: change document filename and move it to a //specific folder  
>
>   MOVE pdfFile INTO FailedPdfsFolder 
>
>      numberOfFailedPdfFiles = numberOfFailedPdfFiles + 1 
>
>      totalNumberOfPdfFiles = totalNumberOfPdfFiles+1 
>
>  ELSE 
>
>    RENAME (oldPdfFileName, newPdfFileName) 
>
>    move pdfFile INTO FinishedPdfsFolder  
>
>      numberOfFinishedPdfFiles = numberOfFinishedPdfFiles + 1 
>
>      totalNumberOfPdfFiles =   totalNumberOfPdfFiles + 1 
>
>  END IF 
>
> END FOR 
>
//8th: send an email to the IT Manager   
>
> SEND EMAIL	 
>
>end. 


![](Images/BPMN.png)

![](Images/DomainModel.JPG)

![](Images/UseCase.JPG)
