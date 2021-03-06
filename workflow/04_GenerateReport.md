# Generate the report/database for DHS/CDC

Adelaide Roguet 3-5-21

Congrats, you are almost done! This last workflow will guide to: 

- Generate the database compiling all ddPCR/qPCR and other metadata, 
- Curate automatically the data to only send the last changes to DHS/CDC, 
- Upload the curated database on the DHS server.



## Before starting

1. Make sure that you flagged all the samples that have to be discarded from the process by adding "need_rerun" in the NeedRerun column. Only good data can be sent to DHS/CDC (see workflow )
2. Make sure all WWTP parameters have been checked (the best is right before starting that step, lessening the chances that someone added poorly formatted data).
3. If the big-spreadsheet is open, make sure it has been saved (to avoid working on an outdated version). Close the big-spreadsheet.
4. Make sure OneDrive Desktop is up-do-date (Figure 1). If not, you will generate a database with out-of-date data!

<iframe src="https://drive.google.com/file/d/1ZvdZwll7P9UcVV4_IjN6iJal-XphV-3T/preview" width="640" height="480"></iframe>
**Figure 1**



## Import the last R scripts from Github

1. Download the Github repository **COVID_Rscripts** located here [Github COVID_Rscripts](https://github.com/AnehEf/COVID_Rscripts/)
   But you can click here to download the scripts:
   [Github R_scripts_download](https://github.com/AnehEf/COVID_Rscripts/archive/master.zip)

2. Unzip the file, and copy the content of the folder **GenerateDatabase** into the directory (OneDrive Desktop has to be installed on your computer):

   > SARS-CoV-2 > REPORTS > CDC_DHS_reports



## Generate the database

*No need to create a folder (with the date and time) to store the new database, the R script will automatically create one for yo. All important files will be stored in it, except a .html file that keeps track of the R processes (that is stored in the CDC_DHS_reports directory).* 

1. Open the file **UWM_SARS-CoV-2_report_script.Rmd** using RStudio
2. Knit the document (File > Knit Document)
3. Once done, cut and paste the .html file generated 

In **CDC_DHS_reports** directory, the script generates a file named **UWM_SARS-CoV-2_report_script.html** and folder named as such:
> **YEAR** - **MONTH** - **DAY** _ **HOURMINUTES**

4. Cut and paste **UWM_SARS-CoV-2_report_script.html** into the folder freshly created.







## Quality control #3

1. Open the file **UWM_SARS-CoV-2_report_script.html** in the freshly created folder
2. Go to the section **Checking section**
3. Check the WWTP parameters input by looking at the table below **```pander(head(metadata.parameter))```**. Make sure it does not contains anomalies.
4. Check the variables classes in the WWTP parameters by looking below **```lapply(metadata.parameter,class)```**. Parameters should be classified as "numeric". If not, make sure it didn't affect the workflow and that everything looks good in the final database.
5. Check the database output by looking at the table below **```pander(head(report))```**. It displays the last samples entered in the database. Make sure it does not contains anomalies, e.g., NA for BCoV, while you know BCoV should be in there, etc..
6. Check that all variable classes are **okay** (Figure 2). If something is wrong, then it should be (kinda) explicit.

<iframe src="https://drive.google.com/file/d/1-mnVTlKaMjVfzxMGhvUq2bPyrb5nhvlr/preview" width="640" height="480"></iframe>
**Figure 2**

7. Visually check N1, N2, N1/N2, PMMoV, and HF183 concentrations across time, as well as the BCoV recovery rate. If any, flag samples that look like outliers. 
- Open **RawData ddPCR** tab in the big spreadsheet
- Add **"need_rerun"** to the column **NeedRerun**
- Delete the folder that has been freshly created (named as e.g., 21-04-05_1558)
- Restart this workflow from the beginning

| If the same values are confirmed with the re-run, then remove all "need_rerun" in the NeedRerun column. Make sure the light grey columns are not displaying "#N/A" any longer.




## Curation of the database
```
The database we send to DHS should:
- not include the samples for which we don't have the N1/N2 and/or average daily flow data
- only contains the samples for which we have new and/or updated values. 
- have no header
- left "NA" cells emptied
- use "|" as separator
```




1. Open **DatabaseCuration.Rmd** using RStudio
   
> SARS-CoV-2 > REPORTS > CDC_DHS_reports > FRESHLY CREATED FOLDER (e.g., 21-04-05_1558) > DatabaseCuration.Rmd

   

2. Set up parameters, for that, add: 
   - Path to the last database sent to CDC/DHS (Figure 3 #1). You only need to add the folder name. 
   - Path to the database you just created in the steps above (Figure 3 #2). You only need to add the folder name. 
   - Database number (based on the last database sent to DHS) (Figure 3 #3)
   
   
   
3. Knit the document (File > Knit Document)

<iframe src="https://drive.google.com/file/d/1izRcWi1_3-ZBgKia2_8ONhjyJHjpQNjC/preview" width="640" height="480"></iframe>

**Figure 3.** In that specific example, the number in the folder **21-3-01_10:30** was **26**




4. Inspection of the file **DatabaseCuration.html**
- It indicates how many samples have been discarded because the N1/N2 (Figure 4 #1) and/or average daily flow (Figure 4 #2) data are missing.
<iframe src="https://drive.google.com/file/d/1mFwNjfPqBq6n3d0zRIZlPgOnovSYxkuI/preview" width="640" height="480"></iframe>
**Figure 4** 


- It indicates how many updated and new samples are in the database that will be sent to DHS (Figure 5)
<iframe src="https://drive.google.com/file/d/1ScoODnt-Gvw_Nbym6koAdMrPD3zLxFZ8/preview" width="640" height="480"></iframe>
**Figure 5**



## Clean up after your mess

1. To avoid confusion, delete the **.Rmd** files that are still in the **CDC_DHS_reports** directory.



## Send the database to the DHS server

1. Copy and paste the last version of the curated database on the big-mac in the folder (a shortcut should be available in the Desktop):
   > HOME (genomics) > cdc_covid_report

   > If your database number is **27**, make sure the last database number is **26**. If not, you have to investigate...
   
   


2. Open FileZilla (Figure 6).

   > The DHS server should be properly configured.



3. Click on Server > Reconnect (Figure 7)



4. Now that you are connected to the DHS server, make sure that you are in the right folder (Figure 8):

>  DHFS > secure > DHS_Covid19 > UWM > PROD

5. On FileZilla, **right click** on the database you want to send, and click on **upload**

   

6. You should now see the file on the right side of the screen. Congrat, you just uploaded the database to the DHS server. 

   

7. Close FileZilla

   

8. Enjoy a cup of coffee or tea!



<iframe src="https://drive.google.com/file/d/1MBXRRVSdhafZ_FOdqdNtkPahCaPBpzR3/preview" width="80" height="80"></iframe>

**Figure 6** 



<iframe src="https://drive.google.com/file/d/1O71UzqpnTQzABRbg0kiJUFUl4QHOTr3h/preview" width="640" height="480"></iframe>
**Figure 7**



<iframe src="https://drive.google.com/file/d/1m5SshZ83YW7j03klZgUaAfVhRF3agxyo/preview" width="640" height="480"></iframe>
**Figure 8**







---

### Notes



Outputs from **UWM_SARS-CoV-2_report_script.Rmd**: 

- 1x directory **RawData** that saves all the files used to generate the database
- 1x file named **uwm_report_2021-03-05_1009.txt** containing the database from all samples collected and processed since August 31, 2020.
- 1x file named **UWM_SARS-CoV-2_report_script.html** summarizing the script to generate the database as well as some quality controls, we will inspect shortly!
- 1x file named **UWM_SARS-CoV-2_report_script.Rmd** to keep the raw .Rmd that has been used to generate the database (just in case).
- 1x file named **DatabaseCuration.Rmd** that we will use shortly to curate the data before sending them to DHS/CDC.





Outputs from **DatabaseCuration.Rmd**:

- 1x file containing the database that will be sent to DHS

> **uwm_report_** YEAR **-** MONTH **-** DAY **-** HOURMINUTE **_** REPORT# **.txt** 

- 1x file listing the samples and the parameters for which updated values have been detected

> **uwm_report_** YEAR **-** MONTH **-** DAY **-** HOURMINUTE **_** REPORT# **_details_changes.txt** 

- 1x file named **DatabaseCuration.html** summarizing the script to generate the database as well as some useful info we will inspect shortly!