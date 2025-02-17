**GetStructuredContentFromPDF_genericJSINExtr.ipynb (Notebook)**

The actual version of the Notebook (GetStructuredContentFromPDF_genericJSONExtr.ipynb) contains one parameter: document_path
The corresponding lakehouse should have a Files/xxx path that is used to store the .pdf files. For example "Files/PDF"
The parameter above should then be set to "Files/PDF/<yourfilename.pdf>

The notebook assumes a Document Intelligence service available that is used to break apart the .pdf files into digestible text for the AOAI model.

The notebook containes a cell that gets secrets form Azure KeyVault. These are the key and region for the Document Intelligence and if necessary
the key and URL for an Azure OpenAI gpt-4-32k model if the Fabric built-in gpt-4-32k model cannot be used. 

Further down the notebook uses a json structure ==> "json_structure" = {}. This variable could also be converted to a parameter to be set from the outside. 
This is also true for the "myjson_schema". This is the python structure that belongs to the "json_structure" above. Future versions could be adjusted in a way
that the python structure is derived from the json structure.

The last user input required is the "new_dfs_info" variable that holds an array of lists that define the tables to be created out of the json_strucure/python structure:
[{"newDataFrameName":"df_xxxxx", "columnNames": ["parsedContent.xxxxx.xxxxx", "parsedContent.xxxxx.xxxxx"...]}] whereas "parsedContent" is the name of the resultset of the 
AOAI request and "xxxx.xxxx" is the json path to the particular column, that should be selected.

Further down the data frames are created according to the array and written out to the default lakehouse as delta tables. 
Once the delta tables are created reports can be created using the semantic model of the lakehouse. 

**PDFExtractStructuredContent.zip (Pipeline)**
The .zip file "PDFExtractStructuredContent.zip" contains the Fabric Data Factory pipeline. To import it create a new pipeline and in the Data Factory editor then import the .zip file. 
To use the pipeline go to the "Get Metadata" activity and adjust the File path and folder by clicking "Browse" in the File path line. 
To make sure that the process finishes the "Sequential" parameter in the settings of the "ForEach" activity is set to true. To run all the files in the source folder in parallel, untick 
the "Sequential" setting of the "ForEach" activity. 

Disclaimer: The objects and materials provided in this repository are offered "as-is" without any guarantees or warranties. By using these resources, you acknowledge that you do so at your own risk. Neither the author nor his employer assumes any responsibility for any consequences arising from the use of these objects.
