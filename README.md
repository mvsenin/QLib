# QLib
QLib functional scripting library implements the following functionality:
- Data Profiling
- Table Operations
- File operations
- Logging
- Qlik server URLs extraction
- Clean up for QLib variables
- Color variables creation
- Data Loading automation with Cron-like logic
- Date Operations
- String Operations

# Special files
1. make.bat - generates QLibFull.qvs with full text of the library
2. QLib.qvs - includes all Qlib scripts
3. QLibFull.qvs - full text of QLib

# Main functionality description

## Data Profiling

### CreateDataProfiling() method

	Desription:
		Implements data profiling procedure calculating data quality measures and KPIs
		Data Profiling logic, universal for any data model. Each measure is calculated for each field individually.

	Parameters:
		- none

	Usage sample:
		Call CreateDataProfiling;
    
## Table Operations

### DropFields() method

	Description:
		Drops fields from the table

	Parameters:
		- @pDropFields_FldList - list of field names delimited with comma
		- @pDropFields_Table - table name to remove fields from

	Usage sample:
		Call DropFields('ZZ, AB', 'MyTable');
  
### DropTable(pDropTable_TableNames) method

	Description:
		Drops tables, if they exist
	
	Parameters:
		- @pDropTable_TableNames - list of Qlik table names delimited with comma
	
	Usage sample:
		Call DropTable('MyTable');

### ListTables() method

	Description:
		ListTables procedure lists all the existing table names and indeces via Trace

	Parameters:
		- none

	Usage sample:
		Call ListTables;

### ShowFields() method

	Description:
		Shows table field list with separator

	Parameters:
		- @pShowFields_TableName - table name

	Usage sample:
		Call ShowFields('MyFavoriteInMemoryTable');
  
### ShowTables() method

	Description:
		Shows table list, delimited by comma

	Parameters:
		- none

	Usage sample:
		 Call ShowTables;
   
### StoreTable() method

	Description:
		Stores Qlik table $(pStoreTable_QlikTableName) as a file $(pStoreTable_FileName)
		with the specified format $(pStoreTable_FileFormat) which is either qvd or txt.

	Parameters:
		- @pStoreTable_QlikTableName - Qlik table name
		- @pStoreTable_FileName - file full path
		- @pStoreTable_FileFormat - file format, it's 'qvd' by default
		- @pStoreTable_NoDrop - flag for drop operation, it's True() by default,
								otherwise it's False()
		- @pStoreTable_ZeroRowStore - flag for zero row tables, it's True() by
								default, otherwise it's False()

	Usage sample:
		Call StoreTable('tmpData', '$(sDestinationFile)', 'txt');

## File operations

### CopyFile() method

	Description:
		CopyFile copies source file to destination one

	Parameters:
		- @pCopyFile_SourceFile - source file name with full path
		- @pCopyFile_DestinationFile - destination file name with full path

	Usage sample:
		Call CopyFile('$(sSourceFile)', '$(sDestinationFile)');
  
### FileExists variable

	Description:
		Checks and returns file presence flag

	Parameters:
		- @$1 - file name with full path and extesion

	Usage sample:
		if $(FileExists($(sOriginFile))) and ($(NewerFile($(sOriginFile), $(sDestinationFile)))) then
		...
		end if

### NewerFile variable

	Description:
		Compares time of origin and destination files and returns True()
		if origin one is newer, otherwise returns False()

	Parameters:
		- @$1 - origin file with full path and extension, text
		- @$2 - destination file, supposed to be rewritten, with full path and extension, text

	Usage sample:
		if $(FileExists($(sOriginFile))) and ($(NewerFile($(sOriginFile), $(sDestinationFile)))) then
		...
		end if

### StoreQVD(pStoreQVD_QlikTableName, pStoreQVD_FileName, pStoreQVD_NoDrop, pStoreQVD_ZeroRowStore) method

	Description:
		Stores Qlik table $(pStoreQVD_QlikTableName) as QVD-file $(pStoreQVD_FileName).

	Parameters:
		- @pStoreQVD_QlikTableName - Qlik table name
		- @pStoreQVD_FileName - file full path
		- @pStoreQVD_NoDrop - flag for drop operation, it's True() by default,
				   otherwise it's False()
	 	- @pStoreQVD_ZeroRowStore - flag for zero row tables, it's True() by
				 default, otherwise it's False()

	Usage sample:
		Call StoreQVD('tmpData', '$(sDestinationFile)');

### LoadQVD(pLoadQVD_FileName, pLoadQVD_QlikTableName, pLoadQVD_LoadMode) method

	Description:
		Loads the specified QVD file $(pLoadQVD_FileName) into Qlik table
		in the $(pLoadQVD_QlikTableName), if specified, otherwise, the QVW file
		name is used as the table name.
		Existing Qlik table can either be replaced (bydefault) or added,
		depending of the mode.
  
	Parameters:
		- @pLoadQVD_FileName - file full path
		- @pLoadQVD_QlikTableName - Qlik table name
		- @pLoadQVD_LoadMode - load mode:
			- r or R - replace (default)
			- a or A - append (concatenate)
	
	Usage sample:
		Call LoadQVD('$(sDestinationFile)', 'tmpData');

## Logging

### ShowNoOfRows(pShowNoOfRows_TableNames, pShowNoOfRows_Comment) method

	Desription:
		Shows nubmer of rows in the table
	
	Parameters:
		- @pShowNoOfRows_TableNames - list of Qlik table names delimited with comma
		- @pShowNoOfRows_Comment - text of optional comment
	
	Usage sample:
		Call ShowNoOfRows('tmpData');

### Trace(pTrace_msg, pTrace_Indent, pTrace_FrameSign) method

	Description:
		Traces message in the frame with indent

	Parameters:
		- @pTrace_msg - message text
		- @pTrace_Indent - indent, 4 by default
		- @pTrace_FrameSign - frame sign, * by default

	Usage sample:
		 Call Trace('Loading data for $(sYearMonth) from all the files...', 1, '=');
	
## URLs

### vQlikServerURL variable

	Description:
		Returns Qlik server name basing on the ComputerName() value

	Usage sample:
		'https://$(=$(vQlikServerURL))/sense/app/'

	Note: ComputerName() value depends on the host the script is executed on,
	      so, in case your publisher server is running on a separate host, you
	      will inevitebly get different values in the task running on
	      the publisher server and in the script editor

## Clean up QLib variables

### QLibCleanUp() method

	Description:
		Cleans up QLib variables

	Parameters:
		- none

	Usage sample:
		Call QLibCleanUp;

## Color Variables

### GenerateColorVariables() method

	Description:
		GenerateColorVariables - loads color codes and generates variables

	Parameters:
		- @sGenerateColorVariables_Mode - generation mode, case insensitive:
    			- Create (or empty) - generate color variables
        		- Clean - clean color variables

	Usage sample:
		Call GenerateColorVariables;
		Call GenerateColorVariables('clean');

## Data Loader

### RunDataLoader() method

	Description:
		RunDataLoader procedure initiates massive data loading according to settings
		provided in the "Data Loader Configuration.xlsx" configuration file, stored
		under the "Data Loader" folder of the source folder
    
    	Dependencies:
		Trace.qvs
		Cron.qvs
		DropTables.qvs
		StoreQVD.qvs
		
	Parameters:
		- @sRunDataLoader_SourceFolder - source folder name
		- @sRunDataLoader_Queue - queue identifier

	Usage sample:
		Call RunDataLoader('lib://Source Folder/Source A');

## Date Operations

### AddMonth() method

	Description:
		AddMonth adds a number of months to the specified variable formatted as YYYYMM

	Parameters:
		- @inVarName - the variabel name
		- @inMonths - number of months to be added, including negative ones

	Usage sample:
		Call AddMonth('sYYYYMM', -12);

### MonthDiff() method

	Description:
		Calculates diff in months beetween two monthes formated as YYYYMM numbers

	Parameters:
 		- @$1 - month #1
		- @$2 - month #2
  
	Usage sample:
		Let sNoOfMonthes = $(MonthDiff(202301, 202201));

## String Operations

### CapitalizeStr() method

	Description:
		Performs string capitalization as per one of the for modes
		
	Parameters:
		- @pCapitalizeStr_StrVarName - string with script variable name
		- @p_Mode - capitalization mode:
			U1L - very first letter to upper, rest to lower
			U1W - first letter of each word to upper, rest to lower (default)
			UP - all to upper
			LO - all to lower
   
	Usage sample:
		Let str = 'ABC  DEF ghi';
		Call CapitalizeStr('str', 'U1w');
