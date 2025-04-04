# CBT449
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 449 is from Keith Moe of Amdahl, and contains their       *   FILE 449
//*           Bookmanager front end package.                        *   FILE 449
//*                                                                 *   FILE 449
//*     Keith E. Moe                                                *   FILE 449
//*     Amdahl Corporation                                          *   FILE 449
//*     1250 E. Arques Ave                                          *   FILE 449
//*     M/S 383                                                     *   FILE 449
//*     Sunnyvale, Ca  94088-3470                                   *   FILE 449
//*     (408) 746-6386                                              *   FILE 449
//*     Keith_Moe@notes.amdahl.com                                  *   FILE 449
//*                                                                 *   FILE 449
//*     Disclaimer                                                  *   FILE 449
//*                                                                 *   FILE 449
//*     As usual, Amdahl Corporation (and I) take no                *   FILE 449
//*     responsibility for how well this works in your              *   FILE 449
//*     environment and any problems it might cause.  Suffice       *   FILE 449
//*     it to say, it does work, as our Users use it regularly.     *   FILE 449
//*     (I also think that it is a good introduction to CGI         *   FILE 449
//*     programming in REXX with the IBM WebServer.)                *   FILE 449
//*                                                                 *   FILE 449
//*        Detailed documentation of this package follows:          *   FILE 449
//*                                                                 *   FILE 449
//*     The Amdahl BookManager BookServer Front-end provides a      *   FILE 449
//*     means to search a "catalog" of all book titles and          *   FILE 449
//*     publication numbers kept in MVS BookManager Sequential      *   FILE 449
//*     Data Sets and link directly to the Book or BookShelf        *   FILE 449
//*     containing the Book.  While obviously dependent on the      *   FILE 449
//*     title of the Book, it eliminates the User having to know    *   FILE 449
//*     which BookShelf contains the Book he or she is looking      *   FILE 449
//*     for.  It supports only Books and BookShelves kept in MVS    *   FILE 449
//*     sequential Data Sets, not anything kept in an HFS.          *   FILE 449
//*                                                                 *   FILE 449
//*     This data Set contains a subset(*) of the Amdahl            *   FILE 449
//*     BookManager Management Utilities.  What is included are     *   FILE 449
//*     those pieces needed to run the BookServer Front-end.        *   FILE 449
//*                                                                 *   FILE 449
//*     List of provided members:                                   *   FILE 449
//*                                                                 *   FILE 449
//*     BKMGRCPS - Assembler Macro                                  *   FILE 449
//*     BKMGRHST - Assembler Macro                                  *   FILE 449
//*     BKMGRINF - Assembler Macro                                  *   FILE 449
//*     BKMGRLOC - Assembler Macro                                  *   FILE 449
//*                                                                 *   FILE 449
//*     BOOKMGRX - BookServer Front-end CGI (WebServer) REXX        *   FILE 449
//*                EXEC                                             *   FILE 449
//*                                                                 *   FILE 449
//*     BKMGRLPA - USERMOD to create LPA resident BookSERVER        *   FILE 449
//*                Load Module                                      *   FILE 449
//*                                                                 *   FILE 449
//*     BOOKCSA  - Started Task JCL to run CCCBKACE (Search Data    *   FILE 449
//*                CSA Load)                                        *   FILE 449
//*     BOOKSACE - JCL to run CCCBKACE (Search Data CSA Load)       *   FILE 449
//*     BOOKSEXT - JCL to build search data from BookShelf          *   FILE 449
//*                List(s) (QLSHELF)                                *   FILE 449
//*     REXXCOMP - JCL to compile the REXX Exec and copy it to      *   FILE 449
//*                an HFS                                           *   FILE 449
//*                                                                 *   FILE 449
//*     CCCBKACE - Assembler Source - Load Search Data into CSA     *   FILE 449
//*     CCCBKCPS - Assembler Source - Cell Pool Subroutine          *   FILE 449
//*     CCCBKDAT - Assembler Source - Data Variable Table           *   FILE 449
//*     CCCBKDEF - Assembler Source - Data Table Lookup             *   FILE 449
//*                Subroutine                                       *   FILE 449
//*     CCCBKDSN - Assembler Source - Data Set Name Generator       *   FILE 449
//*                Subroutine                                       *   FILE 449
//*     CCCBKEXT - Assembler Source - Build Book/BookShelf          *   FILE 449
//*                Extract Data Set                                 *   FILE 449
//*     CCCBKLOC - Assembler Source - Determine location of all     *   FILE 449
//*                Bkmgr Data Sets                                  *   FILE 449
//*     CCCBKPUB - Assembler Source - Format Pub Number             *   FILE 449
//*                Subroutine                                       *   FILE 449
//*     CCCBKSRV - Assembler Source - Search Program called by      *   FILE 449
//*                REXX EXEC                                        *   FILE 449
//*     CCCLOCAT - Assembler Source - LOCATE TSO Command - bonus    *   FILE 449
//*                                                                 *   FILE 449
//*     I assume that you're capable of assembling programs, so     *   FILE 449
//*     I'm not including sample assembly JCL.  Each program is     *   FILE 449
//*     linked by itself.  MACLIB and MODGEN are needed along       *   FILE 449
//*     with the Macros supplied above.                             *   FILE 449
//*                                                                 *   FILE 449
//*     CCCBKCPS, CCCBKDAT, CCCBKDEF, CCCBKDSN, CCCBKPUB, and       *   FILE 449
//*     CCCBKSRV are re-entrant and RMODE ANY. (So is CCCLOCAT,     *   FILE 449
//*     but it's included as a bonus and is not really needed.)     *   FILE 449
//*     CCCBKACE, CCCBKEXT, and CCCBKLOC are "main" programs        *   FILE 449
//*     that are NOT re-rentrant and are RMODE 24 (AMODE 31).       *   FILE 449
//*                                                                 *   FILE 449
//*     Assemble and Link all the programs into a "BookManager      *   FILE 449
//*     Utility" Load Library.  CCCBKEXT is unauthorized and can    *   FILE 449
//*     run from it.  CCCBKACE is AUTHORIZED, so it (and            *   FILE 449
//*     CCCBKCPS which it loads as a subroutine) need to be         *   FILE 449
//*     placed in an authorized library (that end-users should      *   FILE 449
//*     not have access to).  CCCBKSRV should be placed in the      *   FILE 449
//*     LinkList or LPAList, as it is used by the CGI REXX Exec.    *   FILE 449
//*                                                                 *   FILE 449
//*     BOOKMGRX is a REXX Exec that will require installation      *   FILE 449
//*     specific customization (unless you like to see some         *   FILE 449
//*     missing GIFs and bad links).  It (or a complied version     *   FILE 449
//*     of it) needs to end up in a HFS that will be mapped in      *   FILE 449
//*     directives in the WebServer HTTPD.CONF file.                *   FILE 449
//*                                                                 *   FILE 449
//*     Before going any farther....                                *   FILE 449
//*                                                                 *   FILE 449
//*     This BookServer Front-end builds an ECSA Resident Catalog   *   FILE 449
//*     which is anchored using the time-honored technique of       *   FILE 449
//*     using a SubSystem Control Table (SSCT).  The default name   *   FILE 449
//*     of this SubSystem is "BKSV".  This value is EQUated in      *   FILE 449
//*     the BKMGRCSA Macro (Label BKASSNAM).  If you want a         *   FILE 449
//*     different SubSystem Name, change this equate and            *   FILE 449
//*     reassemble CCCBKACE and CCCBKSRV.                           *   FILE 449
//*                                                                 *   FILE 449
//*     There is no attempt made to create the "BKSV" SubSystem     *   FILE 449
//*     Control Table dynamically if it doesn't exist.  You will    *   FILE 449
//*     need to update the IEFSSNxx PARMLIB Member and then         *   FILE 449
//*     either re-IPL or use the SETSSI Command to create it.       *   FILE 449
//*                                                                 *   FILE 449
//*     If you don't like this anchor technique and want to         *   FILE 449
//*     change it, go ahead.                                        *   FILE 449
//*                                                                 *   FILE 449
//*     Building the Book Catalog Data Set                          *   FILE 449
//*                                                                 *   FILE 449
//*     The CCCBKEXT program uses the MVS BookManager "Master"      *   FILE 449
//*     BookShelf List Data Set (specified by the QLSHELF           *   FILE 449
//*     setting in EOXVOPTS REXX Exec) to construct the Book and    *   FILE 449
//*     BookShelf Catalog.  The BookShelf List Data Set Name is     *   FILE 449
//*     specified as a parameter in the JCL used to execute         *   FILE 449
//*     CCCBKEXT.  The output Data Set is VB,259.  Sample JCL is    *   FILE 449
//*     provided in the BOOKSEXT member.  Note that the output      *   FILE 449
//*     needs to be sorted in order to be properly searchable.      *   FILE 449
//*                                                                 *   FILE 449
//*     The sample job is two steps.  The extract is to a           *   FILE 449
//*     temporary Data Set.  The sort of this temporary Data Set    *   FILE 449
//*     is output to a permanent extract Data Set which will be     *   FILE 449
//*     used to create the in storage copy.                         *   FILE 449
//*                                                                 *   FILE 449
//*     Building the In Storage Catalog                             *   FILE 449
//*                                                                 *   FILE 449
//*     The CCCBKACE program use the Book Catalog Data Set to       *   FILE 449
//*     create a Common Storage copy of the Book Catalog (in Key    *   FILE 449
//*     1 Storage) and anchor it in the chosen SSCT.  This          *   FILE 449
//*     eliminates the I/O associated with reading the Catalog      *   FILE 449
//*     Data Set (which in our case is 5 cylinders) for each and    *   FILE 449
//*     every search.                                               *   FILE 449
//*                                                                 *   FILE 449
//*     This program, which is authorized since it needs to         *   FILE 449
//*     obtain CSA, can be run as either a Batch Job or Started     *   FILE 449
//*     Task.  A sample of each is provided.  We runs it as a       *   FILE 449
//*     Started Task specified in our COMMND00 PARMLIB member,      *   FILE 449
//*     so it runs at every IPL on every System.  In addition,      *   FILE 449
//*     it can be run at any time to refresh the in storage Book    *   FILE 449
//*     Catalog whenever updates have been made to the              *   FILE 449
//*     BookManager BookShelf List and the Catalog Data Set         *   FILE 449
//*     rebuilt.  If a previous in storage Book Catalog exists,     *   FILE 449
//*     the old one is freed and the new one built, so there        *   FILE 449
//*     should be no lost CSA.                                      *   FILE 449
//*                                                                 *   FILE 449
//*     BookServer CGI REXX Exec                                    *   FILE 449
//*                                                                 *   FILE 449
//*     The BOOKMGRX REXX Exec and the CCCBKSRV program are the     *   FILE 449
//*     heart of the BookServer Front-end. The REXX Exec (raw or    *   FILE 449
//*     compiled) must be placed into an installation HFS           *   FILE 449
//*     directory that is mapped by an "Exec" directive in the      *   FILE 449
//*     BookServer's HTTPD.CONF file (more on that later).  The     *   FILE 449
//*     sample REXX Compile job shows how to place the compiled     *   FILE 449
//*     REXX Exec (CEXEC) into the chosen HFS.  Whatever name is    *   FILE 449
//*     chosen for the file in the HFS (compiled or not) will be    *   FILE 449
//*     part of the URL to invoke it (and like everything in        *   FILE 449
//*     UNIX is CaSe SeNsItIvE).                                    *   FILE 449
//*                                                                 *   FILE 449
//*     This REXX Exec will need customization for your             *   FILE 449
//*     installation, as it has a whole lot of Amdahl specific      *   FILE 449
//*     GIFs and links.  They are "fairly" isolated, but it's       *   FILE 449
//*     your responsibility to find and fix 'em.  Also, the         *   FILE 449
//*     BookServer URLs are different based on the level of the     *   FILE 449
//*     BookServer you are running and whether or not you have      *   FILE 449
//*     moved the BookServer CGI Load Module (bookmgr.exe) into     *   FILE 449
//*     LPA (as described in my SHARE presentation and included     *   FILE 449
//*     as a Local Mod).  This, too, will have to be changed.       *   FILE 449
//*                                                                 *   FILE 449
//*     The CCCBKSRV program is invoked by the REXX Exec to         *   FILE 449
//*     perform the actual search of the in storage Book Catalog    *   FILE 449
//*     and return the results in a pool of REXX stem variables.    *   FILE 449
//*     Because this program is invoked using the "address          *   FILE 449
//*     LINKMVS" REXX statement, it needs to be available to        *   FILE 449
//*     whatever Address Space the REXX Exec runs in.  The          *   FILE 449
//*     easiest way to accomplish this is to place it in a Link     *   FILE 449
//*     List Data Set or (since it's re-entrant and RMODE ANY)      *   FILE 449
//*     an LPA List Data Set.  This program is unauthorized and     *   FILE 449
//*     needs no special attributes.                                *   FILE 449
//*                                                                 *   FILE 449
//*     HTTPD.CONF Updates                                          *   FILE 449
//*                                                                 *   FILE 449
//*     So, you've assembled the programs, stashed the REXX         *   FILE 449
//*     Exec, and created the in storage Book Catalog.  Now you     *   FILE 449
//*     need to get the BookServer WebServer to invoke the REXX     *   FILE 449
//*     Exec CGI.  To do this, you need to add directives to the    *   FILE 449
//*     HTTPD.CONF (or whatever you've called it) file that is      *   FILE 449
//*     used by the BookServer's WebServer.                         *   FILE 449
//*                                                                 *   FILE 449
//*     You've already had to add "Pass" and "Exec" directives      *   FILE 449
//*     to this file for the BookServer itself, so you (or at       *   FILE 449
//*     least someone in your installation) are somewhat            *   FILE 449
//*     familiar with this process.  For purposes of the samples    *   FILE 449
//*     provided below, let's assume the following:                 *   FILE 449
//*                                                                 *   FILE 449
//*          The main BookServer Front-end Directory is:            *   FILE 449
//*               '/BookServer'                                     *   FILE 449
//*          The CGI BookServer Front-end Directory is:             *   FILE 449
//*               '/BookServer/cgi'                                 *   FILE 449
//*          BookManager Data Sets all start with 'CCCPUBS.'        *   FILE 449
//*               (needed for PDFs)                                 *   FILE 449
//*          The URL "code" for the Front-end CGI is:               *   FILE 449
//*               '/bookmanager-cgi'                                *   FILE 449
//*          The URL "code" for the Front-end files is:             *   FILE 449
//*               '/bookmanager'                                    *   FILE 449
//*          The REXX Exec HFS file name is:                        *   FILE 449
//*               'bookmanager'                                     *   FILE 449
//*                                                                 *   FILE 449
//*     (Again, note the UNIX is very case sensitive.)              *   FILE 449
//*                                                                 *   FILE 449
//*     So stick the following two lines in the HTTPD.CONF after    *   FILE 449
//*     the lines that were inserted for the IBM BookServer:        *   FILE 449
//*                                                                 *   FILE 449
//*     Exec       /bookmanager-cgi/*   /BookServer/cgi/*           *   FILE 449
//*     Pass       /bookmanager/*   /BookServer/*                   *   FILE 449
//*                                                                 *   FILE 449
//*     So given all of the above, a URL of:                        *   FILE 449
//*                                                                 *   FILE 449
//* http://your.mvs.domain.name:portnumber/  (continued next line)  *   FILE 449
//*    bookmanager-cgi/bookmanager/                                 *   FILE 449
//*                                                                 *   FILE 449
//*     will bring up the BookServer Front-end, from which          *   FILE 449
//*     everything else is self-explanatory (yeah, right!).  The    *   FILE 449
//*     trailing slash in the URL is required for some browsers     *   FILE 449
//*     and not for others, but it doesn't hurt to always code      *   FILE 449
//*     it.                                                         *   FILE 449
//*                                                                 *   FILE 449
//*     Assuming that the User has the Adobe Acrobat Plug-in in     *   FILE 449
//*     his or her browser, and that you have uploaded BOOK PDF     *   FILE 449
//*     files to MVS (as binary files into any suitable VB          *   FILE 449
//*     format Data Set) using the same Data Set Name as the        *   FILE 449
//*     BookManager Book with 'PDF' instead of 'BOOK' as the        *   FILE 449
//*     lowest level qualifier, the BookServer Front-end is         *   FILE 449
//*     capable of allowing the User to view the PDF file for a     *   FILE 449
//*     Book which has been located via the search.                 *   FILE 449
//*                                                                 *   FILE 449
//*     To be able to view these PDF files, you need to have a      *   FILE 449
//*     couple of "Service" Directives in the HTTPD.CONF File.      *   FILE 449
//*     Find where IBM stuck the sample commented out "mvsds.so"    *   FILE 449
//*     Service Statement and add the following two statements:     *   FILE 449
//*                                                                 *   FILE 449
//*  Service /bookmanager-pdf/'cccpubs.*  (continued on next line)  *   FILE 449
//*  /usr/lpp/internet/bin/mvsds.so:mvsdsGet/'cccpubs.*             *   FILE 449
//*  Service /bookmanager-pdf/'CCCPUBS.*  (continued on next line)  *   FILE 449
//*  /usr/lpp/internet/bin/mvsds.so:mvsdsGet/'CCCPUBS.*             *   FILE 449
//*                                                                 *   FILE 449
```
