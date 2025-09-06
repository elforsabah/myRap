ADS: com.adobe.ProcessingException: com.adobe.Processin(200101)
Message No. FPRUNX001

Diagnosis
An error occurred when Adobe Document Services (ADS) was launched.

System Response
Error message

Procedure
When troubleshooting, follow the steps described in SAP Note 944221.

First check the connection to ADS. You can use the program FP_PDF_TEST_00 to do this. From SAP NetWeaver Release 7.0, the program FP_CHECK_DESTINATION_SERVICE must also run successfully (both with and without the checkbox selected on the selection screen). If this program does not run successfully, there is a error in the configuration.
