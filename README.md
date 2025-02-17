The actual version of the Notebook contains one parameter: document_path
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
