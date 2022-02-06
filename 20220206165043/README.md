# Useful commands to edit PDF files

## Qpdf command

* `qpdf --empty --pages 1.pdf 2.pdf -- 12.pdf`    Merge the two documents 1.pdf and 2.pdf. The output will be saved to 12.pdf.
* `qpdf --empty --pages 1.pdf 1-2 -- new.pdf`   Write only pages 1 and 2 of 1.pdf. The output will be saved to new.pdf.
* `qpdf --rotate=+90:1 1.pdf 1r.pdf`          Rotate page 1 of 1.pdf 90 degrees clockwise and save to 1r.pdf
* `qpdf --rotate=+90:1-z 1.pdf 1r-all.pdf`     Rotate all pages of 1.pdf 90 degrees clockwise and save to 1r-all.pdf
* `qpdf --encrypt mypw mypw 128 -- public.pdf private.pdf`    Encrypt with 128 bits public.pdf using as the passwd mypw with output as private.pdf
* `qpdf --decrypt --password=mypw private.pdf file-decrypted.pdf`   Decrypt private.pdf with output as file-decrypted.pdf.

## Pdftk command

* `pdftk 1.pdf 2.pdf cat output 12.pdf`   Merge the two documents 1.pdf and 2.pdf. The output will be saved to 12.pdf.
* `pdftk A=1.pdf cat A1-2 output new.pdf`   Write only pages 1 and 2 of 1.pdf. The output will be saved to new.pdf.
* `pdftk A=1.pdf cat A1-endright output new.pdf`    Rotate all pages of 1.pdf 90 degrees clockwise and save result in new.pdf.
* `pdftk public.pdf output private.pdf user_pw PROMPT encrypt pdf`

## Ghostscript utility

* Combine three PDF files into one:  
`gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite  -sOutputFile=all.pdf file1.pdf file2.pdf file3.pdf`
* Split pages 10 to 20 out of a PDF file:  
`gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dDOPDFMARKS=false -dFirstPage=10 -dLastPage=20\
-sOutputFile=split.pdf file.pdf`

## Other tools

* `$ pdfinfo`
It can extract information about PDF files, especially when the files are very large or when a graphical interface is not   available.

* `$ flpsed`
It can add data to a PostScript document. This tool is specifically useful for filling in forms or adding short comments into the document.

* `$ pdfmod`
It is a simple application that provides a graphical interface for modifying PDF documents. Using this tool, you can reorder, rotate, and remove pages; export images from a document; edit the title, subject, and author; add keywords; and combine documents using drag-and-drop action.

Tags:

    #pdf #terminal #merge #edit
