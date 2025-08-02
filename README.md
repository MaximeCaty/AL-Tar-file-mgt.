# AL-Tar-file-mgt.

Fast TAR file reading and writting in pure AL (no DotNet, cloud compatible)
Writting in ustar format with pax header
Created file can handle by windows embeded explorer and other tools such as 7zip
Reading support any ustar format, with or without header.
Creating and reading subfolder way is from the entry name format with "/", such as "report/document.pdf".

This code use stream and temporary blob, total TAR archive must not exceed 2 Go as it is handled in stream that are limited to 2 Go by design in AL.
Have been written with the help of Grok 4 and tested on multiple tar format.

Usage :

  Create/Write :
        CreateTarArchive();
            This initialize a new tar archive with pax header in server memory.

        WriteTarEntry(FileInStream: InStream; EntryName: Text);
            Add a file (entry) to the tar archive

        SaveTarArchive(OutStream: OutStream)

  Read :
        IsTAR(InputInStream: InStream): Boolean
            Return if a file is a readable tar archive format

        OpenTarArchive(TarInStreamParam: InStream)
            Open a tar archive for reading

        GetEntryList(var EntryList: List of [Text])
            Return the list of files (entry) inside the archive previously opened

        ExtractEntry(EntryName: Text; var FileOutStream: OutStream)
            Extract a file (entry) with provided name, into the specified outstream

