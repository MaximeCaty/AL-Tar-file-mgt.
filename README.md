
# AL-Tar-file-mgt.

[](https://github.com/MaximeCaty/AL-Tar-file-mgt.#al-tar-file-mgt)

Fast TAR file reading and writting in pure AL (no DotNet, cloud compatible) Writting in ustar format with pax header Created file can handle by windows embeded explorer and other tools such as 7zip Reading support any ustar format, with or without header. Creating and reading subfolder way is from the entry name format with "/", such as "report/document.pdf".

This code use stream and temporary blob, total TAR archive must not exceed 2 Go as it is handled in stream that are limited to 2 Go by design in AL. Have been written with the help of Grok 4 and tested on multiple tar format.

## Usage

**Create / Write**
    
This initialize a new tar archive with pax header in server memory >

      CreateTarArchive();

 Add a file (entry) to the tar archive   > 

      WriteTarEntry(FileInStream: InStream; EntryName: Text);
   
  Store the tar archive into the specified outstream >

      SaveTarArchive(OutStream: OutStream)

**Read / Extract**

Return if a file is a readable TAR archive format :
        
        IsTAR(InputInStream: InStream): Boolean

  Open a TAR archive stream for reading
        
        OpenTarArchive(TarInStreamParam: InStream)
Return the list of files (entry) contained the archive previously opened
        
        GetEntryList(var EntryList: List of [Text])

Extract a file (entry) with provided name, into the specified outstream
        
        ExtractEntry(EntryName: Text; var FileOutStream: OutStream)

## Example

    var
	    TARMgt: Codeunit "TOO TAR Mgt.";
	    InStr: InStream;
	    OutStr: OutStream;
	    TempBlob: Codeunit "Temp Blob";
	    FilesList: List of [Text];
    begin
	    ...
	    if TARMgt.IsTAR(InStr) then begin
		    TARMgt.GetEntryList(FilesList);
		    if FilesList.Contains('myfile.json') then
			    // Extract a file contained in the TAR to an OutStream :
			    TempBlob.CreateOutStream(OutStr);
			    TARMgt.ExtractEntry('myfile.json', OuStr);
			    Message('File extracted sucessfully, file size : %2 bytes', TempBlob.Length());
			    ...
	    end;
