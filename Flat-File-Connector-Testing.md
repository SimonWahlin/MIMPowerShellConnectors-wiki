Create a text file called InputFile.txt in the connector’s  MAData folder, and then paste the following sample data into it:

> EmployeeID;OfficePhone;MobilePhone;FaxPhone

> 100;425-555-0100;206-555-0101;

> 101;425-555-0120;206-555-0105;312-555-0151

> 102;425-555-0130;;312-555-0171

> 103;425-555-0125;;

> 104;425-555-0127;206-555-0115;312-555-0170

> 105;425-555-0119;;312-555-0101

> 11;425-555-0199;206-555-0175;312-555-0194

By default, the **MAData **folder can be found at:

    %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\MAData\ 

Next, run the **Full Import** run profile for the connector. If successful, the results should look similar to the following screen capture.
![](https://github.com/Microsoft/MIMPowerShellConnectors/blob/master/wiki/FlatFileConnector/Fig0029.jpg)