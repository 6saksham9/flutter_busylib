Database Structure of BUSY

In this document we are going to discuss about the database structure of
BUSY. In a business application, Back-end is used to store the data
entered by the user through Front-end. For storage of information,
database used in BUSY is MS Access or MS SQL Server. Basically, business
applications are based on two entities. The entities are:

-   Master Entity

-   Transaction Entity

As BUSY is a business application, it is also based on the two
above-mentioned entities. Before proceeding further, let us discuss the
basic concept of Master and Transactions entities.

Master Entity

Master entities are created once and stores information that is either
static or that seldom gets changed such as Name, Address, TIN Number and
so on. Information entered in Master entities does not change a great
deal and is used further in transactions. Let us understand the concept
of Master entities in BUSY with the help of an example. Smart clothing,
a garment manufacturing company, creates a party named Rich Styles and
enters its address, TIN No. and other company information in the
*Account* master. As the information regarding the details of the
company rarely changes hence we can refer to it as Master information
and *Account* master as Master Entity.

Transaction Entity

Transactions are day-to-day entries and have a great deal of activity in
them. Transactions entities require frequent posting of records and need
regular updations and insertions. Let us understand the concept of
Transactions entity with the help of above example. Smart Clothing
Company sells goods to Rich Styles Company on regular basis and records
these transactions in the *Sales* voucher. As sales of goods is an
ongoing process hence we can refer the recording of sales in *Sales*
voucher as transactions and *Sales* voucher as Transaction entity.

Let us now discuss about the databases and the tables used for storage
and retrieval of information in BUSY.

Whenever you create a new company in BUSY, two databases are created.
One database is a common database that stores information that is not
year specific and other database has the Beginning Financial year
appended to its name and stores year specific information. Each time,
you switch to a new financial year in the company; a new database is
created with the current year appended to its name. Hence the two types
of database used in BUSY are:

-   Year Specific Database

-   Common Database

Let us first discuss about the tables that are used in Year specific
Database.

Year Specific Database

In the Year Specific database there are three types of tables. We have
classified these tables according to the entities for which they are
used. The three types of tables are:

1.  Master Entity Tables

2.  Transaction Entity Tables

3.  Common Tables

Let us discuss these tables in detail.

1\. Master Entity Tables

All the masters created in BUSY such as Account, Item, Cost Centre and
so on can be referred to as Master entities. Tables that are used for
storing and retrieving information about the Master entities are:

-   Master1

-   MasterSupport

-   Help1

-   MasterAddressInfo

-   Folio1

Let us discuss these tables in detail.

Master 1

Whenever you create a new master, an entry is inserted in the *Master1*
table. A single row is inserted in *Master1* table for each master. Some
of the important fields of the Master1 table are:

-   Code -- This is a unique code given to each master being created.
    This code is not displayed to the user and is used for maintaining
    uniqueness of the records. This is the primary key of the table.

-   Master Type -- Each master has a fixed MasterType. The values to be
    inserted in Master Types are picked from *GlobalConst.bas* file.

-   Name

-   Alias

-   ParentGrp

-   PrintName

-   Stamp -- This field is useful in Multi-user mode to avoid data
    concurrency problem. It implies that if two users read the same data
    at the same time, perform some modifications in the data and one of
    them saves the modified work before the other user, then the second
    user should not be able to save the modifications to data read by
    him. Second user can modify the data only when he reads the data
    again from the database and performs the modification in the newly
    updated data. Initially when a master is created it is given the
    *Stamp* value as 1 and whenever any modifications are made in this
    master its value is incremented by 1. Let us understand this concept
    with the help of an example. A user named *User1* has opened an
    *Account* master *Cash In Hand* and is doing some modification work.
    At the same time *User2* also opens the same account and starts
    doing the modification work. This time both the users have *Stamp*
    value as 1. Now *User1* saves the modified work and the value of
    stamp gets incremented by 1 and becomes 2. At this time when *User2*
    wants to save the modified work, he will not be able to do so as the
    value of *Stamp* that he holds and the one stored in the database
    are different.

-   Creation Date

-   CreatedBy -- It stores the name of the user by whom the master is
    created.

-   ModificationTime -- It stores the date and time on which the last
    modification took place.

-   ModifiedBy -- It stores the name of the user by whom the record was
    last modified.

MasterSupport

*MasterSupport* table acts as a child table to *Master1* table. To
maintain Second Normal Form or we can say to avoid data redundancy this
table is created. Let us understand the need for this table with the
help of an example. A user has created a *TDS Category* master named
*Winning From Game Shows*. With this *TDS Category* master he has
defined five *Payee Categories* having different *Threshold* *Limit*,
*TDS%*, *Surcharge%* and *Cess%*. Now a single row will be created in
the *Master1* table and corresponding five rows will be created in
*MasterSupport* table for different *Payee* *Categories* and their
*Threshold limit*, *TDS%* and so on. Some of the important fields of the
*MasterSupport* table are:

-   MasterCode -- The code of the master to which the corresponding
    entry belongs is stored in this field. This is a foreign key with
    reference to *Master1* table.

-   Date

-   MasterType

-   CM1-2

MasterAddressInfo

All the address related information and the information regarding CST
number, LST number, TIN number get stored in the *MasterAddressInfo*
table. For example, while creating a sundry debtor, address, telephone
number and so on. of the party will be stored in this table. Some of the
important fields of the *MasterAddressInfo* table are:

-   MasterCode

-   Address1

-   Address2

-   CST -- It stores the Central Sales Tax number of the party.

-   LST -- It stores the Local Sales Tax number of the party.

-   TinNo -- It stores the Tax Identification number of the party.

Help1

Whenever a new master is added, its entry is also inserted in *Help1*
table. This table is maintained for faster accessing of records. For
example, while viewing the list of sundry debtors, only those masters
that fall under *Sundry Debto*r category should be listed. In the Help1
table there is a data field named *RecType*, which stores the category
type of master i.e. for all the sundry debtors *RecType* will be 1, for
all sundry creditors *RecType* will be 2 and so on. Now you can easily
specify the query as "select \* from Help1 where RecType= 1". This query
will display the records having account group as Sundry Debtor. This can
be possible with *Master1* table, but the result will be very slow.

Note: The value of RecType to be inserted is picked from GlobalConst.bas
file.

*Help1* table stores individual entries for *Name* and *Alias* of the
Master. This is very useful since while showing the list of masters, we
always show the *Name* followed by *Alias* in the next line. This is not
possible with *Master1* table. Some of the important fields of the
*Help1* table are:

-   RecType -- This field stores the code of the master under which the
    newly created master can be categorised. For example, we have
    created an account master named *Cash In Hand*. Now this account
    master can fall under two different groups that are *Current Asset*
    and *Cash*. The codes for these two masters in *GlobalConst.bas*
    file are 1 and 2 respectively. Now, two entries will be inserted in
    Help1 table with *RecType* 1 and 2.

-   NameAlias

-   Code

-   NameOrAlias -- This field stores the value as 1 if the value stored
    in *NameAlias* field is *Name* of the master and 2 if the value
    stored in the *NameAlias* field is *Alias* of the master.

Folio1

*Folio1* table is used to maintain records of opening balance of
masters. Opening balance can be of an *Account* master, *Item* master,
*Cost Centre* master. For example, while creating a new item if you
specify opening stock of the item then it will get stored in the
*Folio1* table. Some of the important fields of the *Folio1* table are:

-   Code -- This is a foreign key, which has a reference to the *Code*
    field of the *Master1* table.

-   D1-150 -- These are different fields, which store opening balances
    and opening stocks of different masters.

2\. Transaction Entity Tables

All the transactions such as Sale, Sale Return, Stock transfer and so on
can be referred to as Transaction entities. Tables that are used to
store and retrieve information about transaction entities are:

-   Tran1

-   Tran2

-   Tran3

-   Tran4

-   Tran5

-   Tran6

-   Tran7

-   Tran8

-   Tran9

-   Tran10

-   DeletedInfo

-   OEDDet

-   TradingExcise

-   VATInfo

-   VchOtherInfo

-   BillingDet

-   Help2

Let us discuss these tables in detail.

Tran1

A voucher is divided into three parts. First section is Header that
consists of date, Bill No., Party Name. Second section is Body that
consists of Item related information such as item description, price,
quantity and so on. Third section is footer that contains information
about tax, freight and forwarding, discount and so on. *Tran1* table
stores all the information related to the header of a voucher. Some of
the important fields of the *Tran1* table are:

-   VchCode -- This is a unique voucher code given to every voucher.
    This is also the primary key of the table.

-   VchType -- Each voucher has a fix voucher type. These voucher types
    are stored in *GlobalConst.bas* file from where the values are
    picked and inserted in this field. For example, a *Sales* voucher
    has Voucher Type 9; a *Purchase* Voucher has a voucher type 2. Now
    whenever a user adds a *Sales* voucher then 9 will be inserted in
    this field.

-   Date

-   VchNo

-   VchSeriesCode

-   AutoVchNo -- This field is used for auto generation of voucher
    numbering.

-   External -- It stores the value that whether the voucher is entered
    through BUSY or any other Add-On.

-   CreatedBy

-   CreationTime

-   AthourisedBy

-   AthourisationTime

-   ModifiedBy - It stores the name of the user by whom the voucher is
    last modified.

-   ModificationTime -- It stores the date and time on which the record
    was last modified.

Tran2

All the information pertaining to the body and footer of the voucher
such as item related information or Bill Sundry related information gets
stored in this table. Some of the important fields of the *Tran2* table
are:

-   RecType -- It stores the value that whether the inserted entry holds
    item related information or bill sundry related information. The
    values for the *RecType* are picked from *GlobalConst.bas* file. For
    example, if the entry inserted is about the item detail then 1 will
    be inserted in the *RecType*, if the entry inserted is for bill
    sundry then 2 will be inserted in the *RecType* and so on.

-   CashFlow -- It stores the amount that has flowed in or out.

-   ShortNar

-   ClrDate

Tran3

In *Account* Master if you specify any references for opening balances,
and in *Item* Master if you specify any batch details then it will get
stored in the *Tran3* table. *Tran3* table also stores the references
belonging to transactions such as bill references. When you enter a
voucher, a *Bill-by-bill Adjustment of Amount* Window appears.
Information entered through this window gets stored in *Tran3* table.
Some of the important fields of the *Tran3* table are:

-   RefCode -- This is the unique reference code given to each reference
    being created. It is also the Primary Key of the table.

-   RecType

-   Method -- It stores the method i.e. whether the reference being
    created is a new reference or adjusted/appended with any previous
    reference.

-   VchCode

-   DueDate -- It stores the due date of the reference being inserted.

-   Status

-   Type

Tran4

*Tran4* table is used to maintain details of items such as opening stock
in Main Unit/Alternate Unit, opening amount and so on. When you create a
new item then a *Material Centre Wise Opening Stock Detail* window
appears. Information entered through this window gets stored in *Tran4*
table. Some of the important fields of the *Tran4* table are:

-   SrNo -- It stores the serial number of the item.

-   MasterCode1 -- It stores the code of the item.

-   MasterCode2 -- It stores the code of the material centre.

-   D1 -- It stores the opening stock in main unit.

-   D2 -- It stores the opening stock in alternate unit.

-   D3 -- It stores the opening amount.

Tran5

*Tran5* table stores the Cost Centre details. While entering a voucher
an *Allocation of Amount To Cost Centre Window* appears. Information
entered through this window gets stored in *Tran5* table. Some of the
important fields of the *Tran5* table are:

-   MasterCode1 -- It stores the code of the cost centre.

-   TranType -- It stores the value for the type of transaction from
    where the entry has come i.e. whether the entry belongs to *Sales*
    voucher, *Purchase* voucher and so on.

-   ShortNar

Tran 8

*Tran8* table stores the information about the *Forms Received* and
*Forms Issued* documents. It stores only the header information. Some of
the important fields of the *Tran8* table are:

-   FormRecDate -- It stores the date on which form is received from the
    authorities.

-   FormNo

-   AutoVchNo

-   CreatedBy

-   CreationTime

-   AthourisedBy -- It stores the name of the Super User.

-   AthourisationDate -- It stores the date on which Super User has
    given the authority to other user.

-   ModifiedBy

-   ModificationTime

Tran9

*Tran9* table acts as a child table to *Tran8* table. As mentioned
above, details related to the *Form Received/Issued* document or header
information gets stored in *Tran8* table and all the further details
related to forms or the footer information get stored in *Tran9* table.
Some of the important fields of the *Tran9* table are:

-   SrNo. -- It stores the serial number of the entry.

-   Date -- It stores the bill date.

-   BillCode -- It stores the bill code or we can say the voucher number
    against which the form is issued/received.

-   Amount -- It stores the amount receivable/issuable.

DeletedInfo

*DeletedInfo* table stores information about the deleted vouchers. The
entry in this table will be inserted only when you have enabled the
option of *Maintain Data CheckList* in BUSY. This table is helpful if
the company wants to view the details of the deleted vouchers such as
the name of the user who has deleted the voucher and so on. Some of the
important fields of the *DeletedInfo* table are:

-   Identity -- It stores the voucher series, voucher number, voucher
    date of the deleted voucher.

-   DeletedBy

-   DeletionTime

OEDDet

This table stores other excise details entered through *Sale/Purchase
and* *Sale/Purchase Return* vouchers. Some of the important fields of
the *OEDDet* table are:

-   OEDRate

-   OEDAmount

-   CessRate

-   CessAmount

TradingExcise

This table stores the information about *Trading Excise*. If you specify
an opening balance for *Trading Excise* then it will get stored in this
table. At the time of entering a voucher a *RG23D Details* Window
appears. Information entered through this window gets stored in
*TradingExcise* table. Some of the important fields of *TradingExcise*
table are:

-   Method

-   VchNo

-   ItemCode

-   AssValue

-   DutyRate

-   DutyType

VATInfo

This table stores information related to *VAT Journal*. Some of the
important fields of the *VATInfo* table are:

-   Date

-   Amount

-   RecType

-   SrNo.

VchOtherInfo

*VchOtherInfo* table stores the other related information about the
vouchers such as transport details, optional fields and so on. Some of
the important fields of the *VchOtherInfo* table are:

-   Transport

-   Station

-   Form31No

-   Form31Issued

-   GoodsRemovalTime

BillingDet

*BillingDet* table stores information about the party whose billing
details are maintained. While entering a voucher such as *Sale/Purchase*
and so on when you select the name of the *Party* then a *Billing
Details* window appears. In the window you can enter information about
the party with whom you are dealing. The information entered through
this window gets stored in the *BillingDet* table. Some of the important
fields of the *BillingDet* table are:

-   VchCode

-   PartyName

-   Address1

Help2

*Help2* table is maintained for faster accessing of the records.
Information regarding optional fields and transport details get stored
in this table. If optional fields are defined with any master then while
adding a master the details of optional fields also get stored in this
table. Some of the important fields of the *Help2* table are:

-   RecType1 -- It stores a constant signifying whether it is a master
    entry or transaction entry.

-   RecType2

-   RecType3

-   Name -- It stores the value accordingly.

ItemDesc

*ItemDesc* table stores additional item description given with the item
at the time of entering a voucher. While entering a voucher when you
select the name of the item an *Item Add. Field/Description* Window
appears. Information provided through the window gets stored in this
table. Some of the important fields of the *ItemDesc* table are:

-   VchCode

-   SrNo

-   Desc1-4

Excise

*Excise* table stores the details of *Adjust Excise Amounts* document
and the details entered about excise duty at the time of entering
various vouchers. Some of the important fields of the *Excise* table
are:

-   RecType

-   VchCode

-   DocName

-   Value

-   Balance

-   DateOfDeposit

STDet

*STDet* table stores the details related to Service Tax. While entering
a voucher if service tax has been charged/paid on the goods then a
*Service Tax Details* window appears. The information entered through
the window gets stored in this table. Some of the important fields of
the *STDet* table are:

-   VchCode

-   STCategory

-   STAssValue

-   STRate

-   STCessRate

TDS

Information related to TDS details get stored in this table. For
example, while entering a *Payment* voucher, if you are debiting a TDS
duty account then a *TDS Adjust Reference* Window appears. The
information entered through this window gets stored in this table. Some
of the important fields of the *TDS* table are:

-   Method

-   RefNo

-   RefCode

-   MasterCode1

-   MasterCode2

-   EntryDate

-   DateOfCredit

-   SurchargeRate

-   ChallanNo

-   Quarter

3\. Common Tables

Beside above-mentioned tables there are certain more tables that are not
entity specific. We can refer to these tables as common tables. These
tables are:

-   Tran10

-   Config

-   RepOptValue

-   ExternalData

-   Locks

Let us discuss these tables in detail.

Tran10

*Tran10* table is used to store the information about the *Party Item
Price Structure* or *Party Item Discount Structure*. Some of the
important fields of the *Tran10* table are:

-   RecType

-   MasterCode1

-   MasterCode2

Config

*Config* table is used to store information about the different
configuration settings specified by the user. For example, the option of
*Broker* appears in BUSY only when you have enabled the option of
*Broker-wise Reporting* in the *Configuration* menu. Hence, *Config*
serves as a base table from where different values are picked and based
on these values different options appear in BUSY. Some of the important
fields of the *Config* table are:

-   RecType

-   Type

-   NoOfFld

RepOptValue

*RepOptValue* table stores the values specified by the user at the time
of specifying different options for viewing the reports. The values in
this table are maintained only for display purpose. For example, while
viewing *Brokerage Calculation* report, a user specifies the name of the
Broker as Rohan, Brokerage% as 10 and so on. Now whenever the user opens
a *Brokerage calculation* report, the same data will be displayed in the
*Report options* Window. Some of the important fields of the
*RepOptValue* table are:

-   RepId

-   ReportType

-   MasterType

-   FieldId

ExternalData

*ExternalData* table stores a value to specify as to which Add-On, or
external application, the data belongs to. Some of the important fields
of *ExternalData* table are:

-   KeyType

-   KeyCode

-   AppName

-   CanEditData

-   CanDeleteKey

Locks

*Locks* table is helpful in multi-user mode. In a multi-user
environment, if a user is doing data manipulation then *Locks* table is
opened and at that time if any other user wants to do data manipulation
then he will not be able to do so. This is because he will find the
*Locks* table opened and no data manipulation is allowed when *Locks*
table is open.

All the tables of Year Specific Database have been explained. Let us now
discuss the tables used in Common Database

Common Database

Tables under common database store information that is not year
specific. Common Database is the database that stores the information
common to every financial year. For example, if you create a Standard
Narration in a particular financial year then it can be used in any
financial year. Hence, we store Standard Narrations in the common
database. The tables in the Common Database are:

-   Company

-   StdNar

-   UserLog

-   UserPreference

-   UserPreference1

-   PrinterReps

-   ScreenReps

Let us discuss these tables in detail.

Company

*Company* table stores the information related to the company being
created. Some of the important fields in the *Company* table are:

-   Name

-   Print Name

-   Address1-4

-   BegFY -- It stores the beginning financial year of the company.

-   CurFY -- It stores the current financial year of the company.

-   BooksStart -- It store the Books Commencing Date.

-   FreezDate -- It stores the date up to which the data is frozen.

-   LSTNo

-   CSTNo

-   Created On

-   Major

-   Minor

-   Received

StdNar

*StdNar* table stores the Standard Narrations created in BUSY. Some of
the important fields in *StdNar* table are:

-   VchType

-   Nar1 -- It stores the narration entered.

-   Nar2 -- If standard narration is created for accounting vouchers
    such as Payment, Receipt and so on then in BUSY you can specify a
    narration up to 188 characters. First 94 characters will then be
    stored in Nar1 field and the rest of the characters will be stored
    in Nar2 field.

UserLog

*UserLog* table helps in determining whether there is an abnormal
termination of program or not. Whenever a user opens a company, an entry
is inserted in this table and whenever he closes the company, the entry
is deleted from the table. Now if there is an abnormal termination of
the program, the entry will not get deleted from this table. Hence, we
can trace from which system there was an abnormal termination of
program. Some of the important fields of *UserLog* table are:

-   ComputerName

-   BUSYUserName

-   Date

UserPreferences

*UserPreferences* table stores details regarding the Super User and
other users created in the company. For example, modification rights
given to the user are stored in this table. Some of the important fields
of *UserPreferences* table are:

-   Name -- It stores the name of the user.

-   SuperUser -- It stores name of the Super User.

-   CanModifyMaster

-   CanModifyVoucher

-   CanEditCompany

-   DosDrvName

-   WinDrvName

-   PageLen

-   LinesPerPage

UserPreferences1

*UserPreferences1* table stores additional information about the Super
User and other users. Some of the important fields of the
*UserPreferences1* table are:

-   Name

-   C1

PrinterReps

*PrinterReps* table stores details regarding document designing. For
example, if you have created different formats to be used at the time of
printing then details regarding these formats is stored in *PrinterReps*
table. Some of the important fields of *PrinterRep* table are:

-   RepID -- It stores the value to specify the voucher to which the
    format belongs.

-   Name -- It stores the name of the format created.

-   SubRepID -- It stores the value to specify whether the details of
    the format are for Header, Footer, Body, or Page Settings.

-   ScriptLine -- It store the script or the designing part of the
    format.

-   SrNo

ScreenReps

This table is currently not in use.

After explaining the need and relevance of all the tables used in BUSY,
let us now have a look at the different forms, UDTs, classes and tables
used at the time of creation of masters. For your convenience all the
information is summarized in a tabular form.

+--------+---------+---------+--------+--------------------------------+
| ***    | ***Form | ***UDT  | **     | ***Tables Affected***          |
| Master | Name*** | Name*** | *Class |                                |
| N      |         |         | Affec  |                                |
| ame*** |         |         | ted*** |                                |
+--------+---------+---------+--------+--------------------------------+
| A      | frm     | udt     | CA     | 1.  Master1 - Name, Alias,     |
| ccount | AccMast | AccMast | ccMast |     Parent Group, Creation     |
| Master |         |         |        |     date, Modification Date    |
|        |         |         |        |     etc.                       |
|        |         |         |        |                                |
|        |         |         |        | 2.  Folio1 -- Opening          |
|        |         |         |        |     Balances, Budgets, Targets |
|        |         |         |        |     etc.                       |
|        |         |         |        |                                |
|        |         |         |        | 3.  MasterAddressInfo --       |
|        |         |         |        |     Address, TIN No, CST No,   |
|        |         |         |        |     LST No etc.                |
|        |         |         |        |                                |
|        |         |         |        | 4.  Help1 -- Name, Alias       |
|        |         |         |        |                                |
|        |         |         |        | 5.  Help2 -- Optional Fields   |
|        |         |         |        |                                |
|        |         |         |        | 6.  Tran3 -- References of     |
|        |         |         |        |     Opening Balances, Broker   |
|        |         |         |        |     details, Due Date, Amount  |
|        |         |         |        |     etc.                       |
+--------+---------+---------+--------+--------------------------------+
| A      | frm     | udt     | CG     | 1.  Master1 -- Name, Alias,    |
| ccount | GrpMast | GrpMast | rpMast |     Parent Group etc.          |
| Group  |         |         |        |                                |
| Master |         |         |        | 2.  Help1 -- Name, Alias       |
|        |         |         |        |                                |
|        |         |         |        | 3.  Folio1 -- Budgets, Targets |
+--------+---------+---------+--------+--------------------------------+
| Cost   | frm     | udt     | CCo    | 1.  Master1 -- Name, Alias,    |
| Centre | CostCen | CostCen | stCent |     Parent Group etc.          |
|        | treMast | treMast | reMast |                                |
|        |         |         |        | 2.  Help1 -- Name, Alias       |
|        |         |         |        |                                |
|        |         |         |        | 3.  Folio1 -- Opening Balance  |
+--------+---------+---------+--------+--------------------------------+
| Cost   | frm     | udt     | CG     | 1.  Master1 -- Name, Alias,    |
| Centre | GrpMast | GrpMast | rpMast |     Parent Group etc.          |
| Group  |         |         |        |                                |
|        |         |         |        | 2.  Help1 -- Name, Alias       |
+--------+---------+---------+--------+--------------------------------+
| Broker | frmBro  | udtBro  | CBrok  | 1.  Master1 -- Name, Alias     |
|        | kerMast | kerMast | erMast |                                |
|        |         |         |        | 2.  MasterAddressInfo --       |
|        |         |         |        |     Address                    |
|        |         |         |        |                                |
|        |         |         |        | 3.  Help1 -- Name, Alias       |
+--------+---------+---------+--------+--------------------------------+
| Author | frmAut  | udtAut  | CAuth  | 1.  Master1 -- Name, Alias     |
|        | horMast | horMast | orMast |                                |
|        |         |         |        | 2.  MasterAddressInfo --       |
|        |         |         |        |     Address                    |
|        |         |         |        |                                |
|        |         |         |        | 3.  Help1 -- Name, Alias       |
+--------+---------+---------+--------+--------------------------------+
| Std.   | fr      | ud      | C      | 1.  StdNar -- VchType,         |
| Nar    | mSNMast | tSNMast | SNMast |     Narration                  |
| ration |         |         |        |                                |
+--------+---------+---------+--------+--------------------------------+
| Item   | frmI    | udtI    | CIt    | 1.  Master1 -- Name, Alias,    |
|        | temMast | temMast | emMast |     Con factor, main unit, alt |
|        |         |         |        |     unit, author (only first   |
|        |         |         |        |     author specified), stock   |
|        |         |         |        |     valuation method, MRP,     |
|        |         |         |        |     Royalty etc.               |
|        |         |         |        |                                |
|        |         |         |        | 2.  MasterAddressInfo -- Item  |
|        |         |         |        |     Description, Category wise |
|        |         |         |        |     price, discount, value of  |
|        |         |         |        |     optional fields etc.       |
|        |         |         |        |                                |
|        |         |         |        | 3.  Help1 -- Name, Alias       |
|        |         |         |        |                                |
|        |         |         |        | 4.  Help2 -- Optional Fields   |
|        |         |         |        |     etc.                       |
|        |         |         |        |                                |
|        |         |         |        | 5.  Tran3 -- Batch Number,     |
|        |         |         |        |     Material Centre code etc.  |
|        |         |         |        |                                |
|        |         |         |        | 6.  Tran4 -- Opening Stock     |
|        |         |         |        |     (Main/Alt), Opening Amount |
|        |         |         |        |     etc.                       |
|        |         |         |        |                                |
|        |         |         |        | 7.  Folio1 -- Opening stock    |
|        |         |         |        |     qty (main/alt), Opening    |
|        |         |         |        |     Stock amount etc.          |
|        |         |         |        |                                |
|        |         |         |        | 8.  MasterSupport -- Author    |
|        |         |         |        |     (if more than one author   |
|        |         |         |        |     is specified then it       |
|        |         |         |        |     insert data from second    |
|        |         |         |        |     author onwards), royalty   |
|        |         |         |        |     etc.                       |
+--------+---------+---------+--------+--------------------------------+
| Item   | frm     | udt     | CG     | 1.  Master1 -- Name, Alias,    |
| Group  | GrpMast | GrpMast | rpMast |     Parent Group etc.          |
|        |         |         |        |                                |
|        |         |         |        | 2.  Help1 -- Name, Alias       |
+--------+---------+---------+--------+--------------------------------+

+-------+--------+--------+-------+----------------------------------+
| Mat   | frm    | udt    | CM    | 1.  Master1 -- Name, Alias,      |
| erial | MCMast | MCMast | CMast |     Group etc.                   |
| C     |        |        |       |                                  |
| entre |        |        |       | 2.  MasterAddressInfo -- Address |
|       |        |        |       |     etc.                         |
|       |        |        |       |                                  |
|       |        |        |       | 3.  Help1 -- Name, Alias         |
|       |        |        |       |                                  |
|       |        |        |       | 4.  Help2 - Optional fields etc. |
+-------+--------+--------+-------+----------------------------------+
| Mat   | frmG   | udtG   | CGr   | 1.  Master1 -- Name, Alias,      |
| erial | rpMast | rpMast | pMast |     Group                        |
| C     |        |        |       |                                  |
| entre |        |        |       | 2.  Help1 -- Name, Alias         |
| Group |        |        |       |                                  |
+-------+--------+--------+-------+----------------------------------+
| Bill  | frm    | udt    | CB    | 1.  Master1 -- Name, Alias,      |
| S     | BSMast | BSMast | SMast |     Print Name, Sales/Purchase   |
| undry |        |        |       |     Account Code, Percentage of  |
|       |        |        |       |     Amount etc.                  |
|       |        |        |       |                                  |
|       |        |        |       | 2.  Help1 -- Name, Alias         |
+-------+--------+--------+-------+----------------------------------+
| Bill  | frmB   | udtB   | CBO   | 1.  Master1 -- Name, Item, Unit, |
| of    | OMMast | OMMast | MMast |     quantity etc.                |
| Mat   |        |        |       |                                  |
| erial |        |        |       | 2.  Help1 -- Name, Alias         |
|       |        |        |       |                                  |
|       |        |        |       | 3.  MasterSupport -- Raw         |
|       |        |        |       |     Material Used, Items         |
|       |        |        |       |     Generated, Item Type etc.    |
+-------+--------+--------+-------+----------------------------------+
| ST    | frmFo  | udtFo  | CFor  | 1.  Master1 -- Name, Print Name  |
| Form  | rmMast | rmMast | mMast |     etc.                         |
|       |        |        |       |                                  |
|       |        |        |       | 2.  Help1 -- Name, Alias         |
+-------+--------+--------+-------+----------------------------------+
| Sale  | frm    | udt    | CSTP  | 1.  Master1 -- Name, Sales       |
| Type  | STMast | STMast | TMast |     account code, Form           |
|       |        |        |       |     Receivable, Form Issuable,   |
|       |        |        |       |     Tax Bill Sundry, Tax% etc.   |
|       |        |        |       |                                  |
|       |        |        |       | 2.  Help1 -- Name, Alias         |
+-------+--------+--------+-------+----------------------------------+
| Pur   | frm    | udt    | CSTP  | 1.  Master1 -- Name, Purchase    |
| chase | PTMast | PTMast | TMast |     Account Code, Form           |
| Type  |        |        |       |     Receivable, Form Issuable,   |
|       |        |        |       |     Tax Bill Sundry, Tax% etc.   |
|       |        |        |       |                                  |
|       |        |        |       | 2.  Help1 -- Name, Alias         |
+-------+--------+--------+-------+----------------------------------+
| TDS   | frmT   | udtT   | CTD   | 1.  Master1 -- Name, Section     |
| Cat   | DSMast | DSMast | SMast |     Code                         |
| egory |        |        |       |                                  |
|       |        |        |       | 2.  Help1 - Name                 |
|       |        |        |       |                                  |
|       |        |        |       | 3.  MasterSupport -- Threshold   |
|       |        |        |       |     limit, Payee Category, TDS%, |
|       |        |        |       |     CESS%, Surcharge% etc.       |
+-------+--------+--------+-------+----------------------------------+

Let us now look at different forms, UDTs, classes and tables used at the
time of various transactions.

+-------+------+-------+--------+------------------------------------+
| ***Vo | ***  | ***U  | **     | ***Tables Affected***              |
| ucher | Form | DT*** | *Class |                                    |
| Na    | Nam  |       | Affec  |                                    |
| me*** | e*** |       | ted*** |                                    |
+-------+------+-------+--------+------------------------------------+
| Sales | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher Type, Stock   |
| Order | rmIn | hData | ataInv |     Updation Date, Voucher Number, |
|       | vVch |       |        |     Voucher Series Code, Party     |
|       |      |       |        |     Code etc.                      |
|       |      |       |        |                                    |
|       |      |       |        | 2.  Tran2 -- Voucher Number,       |
|       |      |       |        |     Voucher Series Code, Item      |
|       |      |       |        |     Code, Quantity in Main/Alt     |
|       |      |       |        |     Unit, percentage on which      |
|       |      |       |        |     amount is to be calculated,    |
|       |      |       |        |     Nett amount etc.               |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 -- Reference Code,       |
|       |      |       |        |     Method, Due Date, Reference    |
|       |      |       |        |     Number, Reference Status etc.  |
|       |      |       |        |                                    |
|       |      |       |        | 4.  VchOtherInfo -- Voucher Code,  |
|       |      |       |        |     Optional fields (1-10),        |
|       |      |       |        |     Narration etc.                 |
|       |      |       |        |                                    |
|       |      |       |        | 5.  ItemDesc -- Voucher Code,      |
|       |      |       |        |     Serial Number, Item            |
|       |      |       |        |     Description (1-4)              |
|       |      |       |        |                                    |
|       |      |       |        | 6.  Help2 -- Optional Field        |
|       |      |       |        |     number, Series Code, Optional  |
|       |      |       |        |     Field value                    |
+-------+------+-------+--------+------------------------------------+
| Pur   | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher Code, Voucher |
| chase | rmIn | hData | ataInv |     Type, Date, Stock Updation     |
| Order | vVch |       |        |     Date, Created By, Last         |
|       |      |       |        |     Modification Date etc.         |
|       |      |       |        |                                    |
|       |      |       |        | 2.  Tran2 -- Voucher Code, Voucher |
|       |      |       |        |     Number, Date, Percentage value |
|       |      |       |        |     etc                            |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 -- Reference Type,       |
|       |      |       |        |     Method, Reference              |
|       |      |       |        |     Number/Status etc.             |
|       |      |       |        |                                    |
|       |      |       |        | 4.  VchOtherInfo -- Optional       |
|       |      |       |        |     Fields (1-10), Quantity etc.   |
|       |      |       |        |                                    |
|       |      |       |        | 5.  ItemDesc -- Item Description   |
|       |      |       |        |     (1-4)                          |
|       |      |       |        |                                    |
|       |      |       |        | 6.  Help2 -- Voucher Series,       |
|       |      |       |        |     Optional field Number/ Value   |
|       |      |       |        |     etc.                           |
+-------+------+-------+--------+------------------------------------+
| Sale  | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher               |
|       | rmIn | hData | ataInv |     Type/Number/Series Code, Stock |
|       | vVch |       |        |     Updation Date, Sale Type Code, |
|       |      |       |        |     Form Received/Issued-- Status  |
|       |      |       |        |     /Code/ Amount, Excise --       |
|       |      |       |        |     Applicable / Value/ Type,      |
|       |      |       |        |     Broker Code etc.               |
|       |      |       |        |                                    |
|       |      |       |        | 2.  Tran2 -- Material Centre Code, |
|       |      |       |        |     Bill Sundry Master Code,       |
|       |      |       |        |     Account Master Code, Cash      |
|       |      |       |        |     Flow, Item Additional Info,    |
|       |      |       |        |     Opening Balance amount etc.    |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 -- Reference Code,       |
|       |      |       |        |     Method, Due Date, Broker       |
|       |      |       |        |     Number, Item Code etc.         |
|       |      |       |        |                                    |
|       |      |       |        | 4.  Tran5 -- Voucher code/ Type/   |
|       |      |       |        |     Series Code, Cost Centre Code, |
|       |      |       |        |     Serial Number, Short Narration |
|       |      |       |        |     etc.                           |
|       |      |       |        |                                    |
|       |      |       |        | 5.  VchOtherInfo -- Voucher Code,  |
|       |      |       |        |     Optional Fields (1-10),        |
|       |      |       |        |     Transport Details, Goods       |
|       |      |       |        |     Removal Time, Invoice          |
|       |      |       |        |     Preparation Time, Total        |
|       |      |       |        |     Quantity etc.                  |
|       |      |       |        |                                    |
|       |      |       |        | 6.  OEDDet -- Other Excise Duty    |
|       |      |       |        |     Rate/ Amount etc.              |
|       |      |       |        |                                    |
|       |      |       |        | 7.  ItemDesc -- Serial Number,     |
|       |      |       |        |     Item Description (1-4) etc.    |
|       |      |       |        |                                    |
|       |      |       |        | 8.  Help2 -- Series Code, Optional |
|       |      |       |        |     Field Number/ Value etc.       |
|       |      |       |        |                                    |
|       |      |       |        | 9.  Folio1                         |
|       |      |       |        |                                    |
|       |      |       |        | 10. Excise -- BED/AED Amount,      |
|       |      |       |        |     balances etc.                  |
|       |      |       |        |                                    |
|       |      |       |        | 11. BillingDet -- Voucher Code,    |
|       |      |       |        |     Party Name, Address (1-4)      |
|       |      |       |        |                                    |
|       |      |       |        | 12. STDet -- Voucher Code, Service |
|       |      |       |        |     Tax Category, Rate etc.        |
|       |      |       |        |                                    |
|       |      |       |        | 13. TradingExcise -- Reference     |
|       |      |       |        |     No., Duty Amount etc.          |
+-------+------+-------+--------+------------------------------------+

+-------+------+-------+--------+------------------------------------+
| Pur   | f    | udtVc | CVchD  | 1.  Tran1 - Voucher                |
| chase | rmIn | hData | ataInv |     Type/Number/Series Code, Stock |
|       | vVch |       |        |     Updation Date, Sale Type Code, |
|       |      |       |        |     Form Received/Issued-- Status  |
|       |      |       |        |     /Code/ Amount, Excise --       |
|       |      |       |        |     Applicable / Value/ Type,      |
|       |      |       |        |     Broker Code etc.               |
|       |      |       |        |                                    |
|       |      |       |        | 2.  Tran2 - Material Centre Code,  |
|       |      |       |        |     Bill Sundry Master Code,       |
|       |      |       |        |     Account Master Code, Cash      |
|       |      |       |        |     Flow, Item Additional Info,    |
|       |      |       |        |     Opening Balance amount etc.    |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 - Reference Code,        |
|       |      |       |        |     Method, Due Date, Broker       |
|       |      |       |        |     Number, Item Code etc.         |
|       |      |       |        |                                    |
|       |      |       |        | 4.  Tran5 - Voucher code/ Type/    |
|       |      |       |        |     Series Code, Cost Centre Code, |
|       |      |       |        |     Serial Number, Short Narration |
|       |      |       |        |     etc.                           |
|       |      |       |        |                                    |
|       |      |       |        | 5.  VchOtherInfo -- Optional       |
|       |      |       |        |     Fields (1-10), Transport       |
|       |      |       |        |     Details, Form 31 Number/       |
|       |      |       |        |     Issued Date, Purchase Bill     |
|       |      |       |        |     Number/ Date etc.              |
|       |      |       |        |                                    |
|       |      |       |        | 6.  TradingExcise -- Reference     |
|       |      |       |        |     Code/ Number, Method, Duty     |
|       |      |       |        |     Rate/ Type/ Amount, balance,   |
|       |      |       |        |     Manufacturer Details (Bill     |
|       |      |       |        |     Number, Quantity, Code) etc.   |
|       |      |       |        |                                    |
|       |      |       |        | 7.  OEDDet -- Voucher Code, AED    |
|       |      |       |        |     Amount                         |
|       |      |       |        |                                    |
|       |      |       |        | 8.  ItemDesc -- Voucher Code, Item |
|       |      |       |        |     Description (1-4)              |
|       |      |       |        |                                    |
|       |      |       |        | 9.  Help2 -- Transport Details,    |
|       |      |       |        |     Item Description etc.          |
|       |      |       |        |                                    |
|       |      |       |        | 10. Folio1                         |
|       |      |       |        |                                    |
|       |      |       |        | 11. Excise -- Voucher Number,      |
|       |      |       |        |     BED/AED -- Amount/ Balance     |
|       |      |       |        |     etc.                           |
|       |      |       |        |                                    |
|       |      |       |        | 12. BillingDet -- Voucher Code,    |
|       |      |       |        |     Party Name/ Address            |
|       |      |       |        |                                    |
|       |      |       |        | 13. STDet -- Voucher Code, Service |
|       |      |       |        |     Tax Category, Rate etc         |
+-------+------+-------+--------+------------------------------------+
| Sale  | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher               |
| R     | rmIn | hData | ataInv |     Type/Number/Series Code, Stock |
| eturn | vVch |       |        |     Updation Date, Sale Type Code, |
|       |      |       |        |     Form Received/Issued-- Status  |
|       |      |       |        |     /Code/ Amount, Excise --       |
|       |      |       |        |     Applicable / Value/ Type,      |
|       |      |       |        |     Broker Code etc.               |
|       |      |       |        |                                    |
|       |      |       |        | 2.  Tran2 - Material Centre Code,  |
|       |      |       |        |     Bill Sundry Master Code,       |
|       |      |       |        |     Account Master Code, Cash      |
|       |      |       |        |     Flow, Item Additional Info,    |
|       |      |       |        |     Opening Balance amount etc.    |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 - Reference Code,        |
|       |      |       |        |     Method, Due Date, Broker       |
|       |      |       |        |     Number, Item Code etc.         |
|       |      |       |        |                                    |
|       |      |       |        | 4.  Tran5 - Voucher code/ Type/    |
|       |      |       |        |     Series Code, Cost Centre Code, |
|       |      |       |        |     Serial Number, Short Narration |
|       |      |       |        |     etc.                           |
|       |      |       |        |                                    |
|       |      |       |        | 5.  VchOtherInfo - Voucher Code,   |
|       |      |       |        |     Optional Fields (1-10),        |
|       |      |       |        |     Transport Details, Form 31     |
|       |      |       |        |     umber etc.                     |
|       |      |       |        |                                    |
|       |      |       |        | 6.  OEDDet -- Voucher Code, AED    |
|       |      |       |        |     Amount                         |
|       |      |       |        |                                    |
|       |      |       |        | 7.  Help2 - Transport Details,     |
|       |      |       |        |     Item Description etc.          |
|       |      |       |        |                                    |
|       |      |       |        | 8.  Excise - -- Voucher Number,    |
|       |      |       |        |     BED/AED -- Amount/ Balance     |
|       |      |       |        |     etc.                           |
|       |      |       |        |                                    |
|       |      |       |        | 9.  BillingDet - Voucher Code,     |
|       |      |       |        |     Party Name/ Address            |
|       |      |       |        |                                    |
|       |      |       |        | 10. STDet -- Voucher Code, Service |
|       |      |       |        |     Tax Category, Rate etc.        |
|       |      |       |        |                                    |
|       |      |       |        | 11. ItemDesc -- Voucher Code, Item |
|       |      |       |        |     Description                    |
|       |      |       |        |                                    |
|       |      |       |        | 12. TradingExcise -- Reference     |
|       |      |       |        |     No., Duty Amount etc.          |
+-------+------+-------+--------+------------------------------------+

+-------+------+-------+--------+------------------------------------+
| Pur   | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher               |
| chase | rmIn | hData | ataInv |     Type/Number/Series Code, Stock |
| R     | vVch |       |        |     Updation Date, Sale Type Code, |
| eturn |      |       |        |     Form Received/Issued-- Status  |
|       |      |       |        |     /Code/ Amount, Excise --       |
|       |      |       |        |     Applicable / Value/ Type,      |
|       |      |       |        |     Broker Code etc.               |
|       |      |       |        |                                    |
|       |      |       |        | 2.  Tran2 - Material Centre Code,  |
|       |      |       |        |     Bill Sundry Master Code,       |
|       |      |       |        |     Account Master Code, Cash      |
|       |      |       |        |     Flow, Item Additional Info,    |
|       |      |       |        |     Opening Balance amount etc.    |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 - Reference Code,        |
|       |      |       |        |     Method, Due Date, Broker       |
|       |      |       |        |     Number, Item Code etc.         |
|       |      |       |        |                                    |
|       |      |       |        | 4.  Tran5 - Voucher code/ Type/    |
|       |      |       |        |     Series Code, Cost Centre Code, |
|       |      |       |        |     Serial Number, Short Narration |
|       |      |       |        |     etc.                           |
|       |      |       |        |                                    |
|       |      |       |        | 5.  VchOtherInfo - Voucher Code,   |
|       |      |       |        |     Optional Fields (1-10),        |
|       |      |       |        |     Transport Details, Form 31     |
|       |      |       |        |     umber etc.                     |
|       |      |       |        |                                    |
|       |      |       |        | 6.  OEDDet -- Voucher Code, AED    |
|       |      |       |        |     Amount                         |
|       |      |       |        |                                    |
|       |      |       |        | 7.  Help2 - Transport Details,     |
|       |      |       |        |     Item Description etc.          |
|       |      |       |        |                                    |
|       |      |       |        | 8.  Excise - Voucher Number,       |
|       |      |       |        |     BED/AED -- Amount/ Balance     |
|       |      |       |        |     etc.                           |
|       |      |       |        |                                    |
|       |      |       |        | 9.  BillingDet - Voucher Code,     |
|       |      |       |        |     Party Name/ Address            |
|       |      |       |        |                                    |
|       |      |       |        | 10. STDet -- Voucher Code, Service |
|       |      |       |        |     Tax Category, Rate etc         |
|       |      |       |        |                                    |
|       |      |       |        | 11. ItemDesc -- Voucher Code, Item |
|       |      |       |        |     Description                    |
|       |      |       |        |                                    |
|       |      |       |        | 12. TradingExcise -- Reference     |
|       |      |       |        |     No., Duty Amount etc.          |
+-------+------+-------+--------+------------------------------------+
| Pa    | frm  | udtVc | CVchD  | 1.  Tran1 -- Voucher               |
| yment | Jou  | hData | ataAcc |     Code/Type/Series Code, Excise  |
|       | rnal |       |        |     Applicable/Doc Name/ Value,    |
|       | Vch  |       |        |     Created By, Modified By etc.   |
|       |      |       |        |                                    |
|       |      |       |        | 2.  Tran2 -- Account Master/ Item  |
|       |      |       |        |     Master Code, Material Centre   |
|       |      |       |        |     code etc.                      |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 -- Reference -- Code/    |
|       |      |       |        |     Due Date/ Status, Broker Code  |
|       |      |       |        |     etc.                           |
|       |      |       |        |                                    |
|       |      |       |        | 4.  Tran5 -- Voucher Code/Series   |
|       |      |       |        |     Code, Cost Centre Code,        |
|       |      |       |        |     Transaction Type, short        |
|       |      |       |        |     Narration etc.                 |
|       |      |       |        |                                    |
|       |      |       |        | 5.  VchOtherInfo -- Voucher Code,  |
|       |      |       |        |     Optional Fields (1-10) etc.    |
|       |      |       |        |                                    |
|       |      |       |        | 6.  Help2 -- Optional field        |
|       |      |       |        |     value/Type etc.                |
|       |      |       |        |                                    |
|       |      |       |        | 7.  Excise -- Amount, Balance etc. |
|       |      |       |        |                                    |
|       |      |       |        | 8.  STDet -- Voucher Code, Service |
|       |      |       |        |     Tax Category, Rate etc         |
|       |      |       |        |                                    |
|       |      |       |        | 9.  TDS -- Reference Number, TDS   |
|       |      |       |        |     Rate/Amount, Surcharge         |
|       |      |       |        |     Rate/Amount, Challan Number,   |
|       |      |       |        |     Quarter etc.                   |
+-------+------+-------+--------+------------------------------------+

+-------+---+------+-------+--------+------------------------------------+
| Re    | f |      | udtVc | CVchD  | 1.  Tran1 -- Voucher               |
| ceipt | r |      | hData | ataAcc |     Code/Type/Series Code, Excise  |
|       | m |      |       |        |     Applicable/Doc Name/ Value,    |
|       | J |      |       |        |     Created By, Modified By etc.   |
|       | o |      |       |        |                                    |
|       | u |      |       |        | 2.  Tran2 -- Account Master/ Item  |
|       | r |      |       |        |     Master Code, Material Centre   |
|       | n |      |       |        |     code etc.                      |
|       | a |      |       |        |                                    |
|       | l |      |       |        | 3.  Tran3 -- Reference -- Code/    |
|       | V |      |       |        |     Due Date/ Status, Broker Code  |
|       | c |      |       |        |     etc.                           |
|       | h |      |       |        |                                    |
|       |   |      |       |        | 4.  Tran5 -- Voucher Code/Series   |
|       |   |      |       |        |     Code, Cost Centre Code,        |
|       |   |      |       |        |     Transaction Type, short        |
|       |   |      |       |        |     Narration etc.                 |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 5.  VchOtherInfo -- Voucher Code,  |
|       |   |      |       |        |     Optional Fields (1-10) etc.    |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 6.  Help2 -- Optional field        |
|       |   |      |       |        |     value/Type etc.                |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 7.  Excise -- Amount, Balance etc. |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 8.  STDet -- Voucher Code, Service |
|       |   |      |       |        |     Tax Category, Rate etc         |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 9.  TDS -- Reference Number, TDS   |
|       |   |      |       |        |     Rate/Amount, Surcharge         |
|       |   |      |       |        |     Rate/Amount, Challan Number,   |
|       |   |      |       |        |     Quarter etc.                   |
+-------+---+------+-------+--------+------------------------------------+
| Jo    | f |      | udtVc | CVchD  | 1.  Tran1 -- Voucher -- Code/      |
| urnal | r |      | hData | ataAcc |     Type/Series Code, Auto voucher |
|       | m |      |       |        |     Numbering, Created By,         |
|       | J |      |       |        |     Modified By etc.               |
|       | o |      |       |        |                                    |
|       | u |      |       |        | 2.  Tran2 -- Account Master Code,  |
|       | r |      |       |        |     tem Master Code, material      |
|       | n |      |       |        |     Centre Code etc.               |
|       | a |      |       |        |                                    |
|       | l |      |       |        | 3.  Tran3 -- Reference Code, Due   |
|       | V |      |       |        |     Date, Broker Code etc.         |
|       | c |      |       |        |                                    |
|       | h |      |       |        | 4.  Tran5 -- Cost Centre Code,     |
|       |   |      |       |        |     Transaction Type, Short        |
|       |   |      |       |        |     Narration etc.                 |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 5.  VchOtherInfo -- Optional       |
|       |   |      |       |        |     fields (1-10) etc.             |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 6.  Help2 -- Optional Field        |
|       |   |      |       |        |     Number, Value etc.             |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 7.  STDet -- Voucher Code, Service |
|       |   |      |       |        |     Tax Category, Rate etc         |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 8.  TDS -- Reference Number, TDS   |
|       |   |      |       |        |     Rate/Amount, Surcharge         |
|       |   |      |       |        |     Rate/Amount, Challan Number,   |
|       |   |      |       |        |     Quarter etc.                   |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 9.  Excise -- Vchcode, DocName,    |
|       |   |      |       |        |     Date                           |
+-------+---+------+-------+--------+------------------------------------+
| C     |   | f    | udtVc | CVchD  | 1.  Tran1 - Voucher -- Code/       |
| ontra |   | rmJo | hData | ataAcc |     Type/Series Code, Auto voucher |
|       |   | urna |       |        |     Numbering, Created By,         |
|       |   | lVch |       |        |     Modified By etc.               |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 2.  Tran2 -- Account Master Code,  |
|       |   |      |       |        |     Date, cash Flow etc.           |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 3.  VchOtherInfo -- Optional       |
|       |   |      |       |        |     fields (1-10)                  |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 4.  Help2 -- Optional Field        |
|       |   |      |       |        |     Number/ Value etc.             |
+-------+---+------+-------+--------+------------------------------------+
| Debit |   | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher -- Code/      |
| Note  |   | rmJo | hData | ataAcc |     Type/Series Code, Auto voucher |
|       |   | urna |       |        |     Numbering, Created By,         |
|       |   | lVch |       |        |     Modified By etc.               |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 2.  Tran2 -- Account Master Code,  |
|       |   |      |       |        |     tem Master Code, material      |
|       |   |      |       |        |     Centre Code etc.               |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 3.  Tran3 -- Reference Code, Due   |
|       |   |      |       |        |     Date, Broker Code etc.         |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 4.  Tran5 -- Cost Centre Code,     |
|       |   |      |       |        |     Transaction Type, Short        |
|       |   |      |       |        |     Narration etc.                 |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 5.  VchOtherInfo -- Optional       |
|       |   |      |       |        |     fields (1-10) etc.             |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 6.  Help2 -- Optional Field        |
|       |   |      |       |        |     Number, Value etc.             |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 7.  STDet -- Voucher Code, Service |
|       |   |      |       |        |     Tax Category, Rate etc         |
|       |   |      |       |        |                                    |
|       |   |      |       |        | 8.  Excise -- Vchcode, DocName,    |
|       |   |      |       |        |     Date                           |
+-------+---+------+-------+--------+------------------------------------+

+-------+------+-------+--------+------------------------------------+
| C     | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher -- Code/      |
| redit | rmJo | hData | ataAcc |     Type/Series Code, Auto voucher |
| Note  | urna |       |        |     Numbering, Created By,         |
|       | lVch |       |        |     Modified By etc.               |
|       |      |       |        |                                    |
|       |      |       |        | 2.  Tran2 -- Account Master Code,  |
|       |      |       |        |     tem Master Code, material      |
|       |      |       |        |     Centre Code etc.               |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 -- Reference Code, Due   |
|       |      |       |        |     Date, Broker Code etc.         |
|       |      |       |        |                                    |
|       |      |       |        | 4.  Tran5 -- Cost Centre Code,     |
|       |      |       |        |     Transaction Type, Short        |
|       |      |       |        |     Narration etc.                 |
|       |      |       |        |                                    |
|       |      |       |        | 5.  VchOtherInfo -- Optional       |
|       |      |       |        |     fields (1-10) etc.             |
|       |      |       |        |                                    |
|       |      |       |        | 6.  Help2 -- Optional Field        |
|       |      |       |        |     Number, Value etc.             |
|       |      |       |        |                                    |
|       |      |       |        | 7.  STDet -- Voucher Code, Service |
|       |      |       |        |     Tax Category, Rate etc         |
|       |      |       |        |                                    |
|       |      |       |        | 8.  Excise -- Vchcode, DocName,    |
|       |      |       |        |     Date                           |
+-------+------+-------+--------+------------------------------------+
| Stock | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher Code/Type,    |
| Tra   | rmIn | hData | ataInv |     Material Centre (From/To) etc. |
| nsfer | vVch |       |        |                                    |
|       |      |       |        | 2.  Tran2 -- Item Code, Material   |
|       |      |       |        |     centre (To/ From), Bill Sundry |
|       |      |       |        |     Master Code, Main/Alt Unit     |
|       |      |       |        |     Code, Tax Paid, Exempted,      |
|       |      |       |        |     Percent Value etc.             |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 -- Reference Code,       |
|       |      |       |        |     Method, Item Code, Material    |
|       |      |       |        |     Centre (From/To), Batch        |
|       |      |       |        |     Number, Due Date etc.          |
|       |      |       |        |                                    |
|       |      |       |        | 4.  VchOtherInfo -- Optional       |
|       |      |       |        |     Fields, Transport Details,     |
|       |      |       |        |     Narration etc.                 |
|       |      |       |        |                                    |
|       |      |       |        | 5.  ItemDesc -- Serial Number,     |
|       |      |       |        |     Item Description               |
|       |      |       |        |                                    |
|       |      |       |        | 6.  Help2 -- Transport Details     |
|       |      |       |        |     etc.                           |
+-------+------+-------+--------+------------------------------------+
| Produ | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher Code/Type,    |
| ction | rmPr | hData | ataInv |     Date, Stock Updation Date, BOM |
|       | oduc |       |        |     Name etc.                      |
|       | tion |       |        |                                    |
|       |      |       |        | 2.  Tran2 -- Item Code, Material   |
|       |      |       |        |     Centre Code, Quantity in       |
|       |      |       |        |     Main/Alt Unit, BOM Code etc.   |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 -- Reference Code, Item  |
|       |      |       |        |     Code, Material centre Code,    |
|       |      |       |        |     Due date etc.                  |
|       |      |       |        |                                    |
|       |      |       |        | 4.  VchOtherInfo -- Optional       |
|       |      |       |        |     Fields                         |
|       |      |       |        |                                    |
|       |      |       |        | 5.  ItemDesc -- Voucher Code, Item |
|       |      |       |        |     Description                    |
|       |      |       |        |                                    |
|       |      |       |        | 6.  Help2 -- Optional field        |
|       |      |       |        |     Number/value                   |
+-------+------+-------+--------+------------------------------------+
| Unass | f    | udtVc | CVchD  | 1.  Tran1 -- Voucher Code/Type,    |
| emble | rmPr | hData | ataInv |     Date, Stock Updation Date, BOM |
|       | oduc |       |        |     Name etc.                      |
|       | tion |       |        |                                    |
|       |      |       |        | 2.  Tran2 -- Item Code, Material   |
|       |      |       |        |     Centre Code, Quantity in       |
|       |      |       |        |     Main/Alt Unit, BOM Code etc.   |
|       |      |       |        |                                    |
|       |      |       |        | 3.  Tran3 -- Reference Code, Item  |
|       |      |       |        |     Code, Material centre Code,    |
|       |      |       |        |     Due date etc.                  |
|       |      |       |        |                                    |
|       |      |       |        | 4.  VchOtherInfo -- Optional       |
|       |      |       |        |     Fields                         |
|       |      |       |        |                                    |
|       |      |       |        | 5.  ItemDesc -- Voucher Code, Item |
|       |      |       |        |     Description                    |
|       |      |       |        |                                    |
|       |      |       |        | 6.  Help2 -- Optional field        |
|       |      |       |        |     Number/value                   |
+-------+------+-------+--------+------------------------------------+

+--------+-----------+---------+----------+---------------------------+
| Stock  | frmP      | udt     | CVc      | 1)  Tran1 -- Voucher      |
| J      | roduction | VchData | hDataInv |     Code/Type, Material   |
| ournal |           |         |          |     Centre Code, Auto     |
|        |           |         |          |     Voucher Numbering     |
|        |           |         |          |     etc.                  |
|        |           |         |          |                           |
|        |           |         |          | 2)  Tran2 - Item Code,    |
|        |           |         |          |     Material Centre Code, |
|        |           |         |          |     Quantity in Main/Alt  |
|        |           |         |          |     Unit, BOM Code etc.   |
|        |           |         |          |                           |
|        |           |         |          | 3)  Tran3 -- Reference    |
|        |           |         |          |     Code, Item Code,      |
|        |           |         |          |     Material centre Code, |
|        |           |         |          |     Due date, Batch       |
|        |           |         |          |     Number etc            |
|        |           |         |          |                           |
|        |           |         |          | 4)  VchOtherInfo --       |
|        |           |         |          |     Optional Fields       |
|        |           |         |          |                           |
|        |           |         |          | 5)  ItemDesc - Voucher    |
|        |           |         |          |     Code, Item            |
|        |           |         |          |     description           |
|        |           |         |          |                           |
|        |           |         |          | 6)  Help2 -- Optional     |
|        |           |         |          |     Field Number/Value    |
|        |           |         |          |     etc.                  |
+--------+-----------+---------+----------+---------------------------+
| Mat.   | frmInvVch | udt     | CVc      | 1.  Tran1 -- Voucher      |
| Rcvd.  |           | VchData | hDataInv |     Code/Type/Number,     |
| From   |           |         |          |     Party Code, Stock     |
| Party  |           |         |          |     Updation date etc.    |
|        |           |         |          |                           |
|        |           |         |          | 2.  Tran2 -- Voucher      |
|        |           |         |          |     Code, Item code, Bill |
|        |           |         |          |     Sundry master Code,   |
|        |           |         |          |     Quantity in Main/alt  |
|        |           |         |          |     Unit etc.             |
|        |           |         |          |                           |
|        |           |         |          | 3.  Tran3 -- Reference    |
|        |           |         |          |     Code, Material Centre |
|        |           |         |          |     code, Item Code,      |
|        |           |         |          |     Batch Number, Status, |
|        |           |         |          |     due Date etc.         |
|        |           |         |          |                           |
|        |           |         |          | 4.  VchOtherInfo --       |
|        |           |         |          |     Optional fields,      |
|        |           |         |          |     Transport Details     |
|        |           |         |          |                           |
|        |           |         |          | 5.  Help2 -- Transport    |
|        |           |         |          |     Detail                |
|        |           |         |          |                           |
|        |           |         |          | 6.  ItemDesc -- Voucher   |
|        |           |         |          |     Code, Item            |
|        |           |         |          |     Description           |
+--------+-----------+---------+----------+---------------------------+
| Mat.   | frmInvVch | udt     | CVc      | 1.  Tran1 -- Voucher      |
| Issued |           | VchData | hDataInv |     Code/Type/Number,     |
| To     |           |         |          |     Party Code, Stock     |
| Party  |           |         |          |     Updation date etc.    |
|        |           |         |          |                           |
|        |           |         |          | 2.  Tran2 -- Voucher      |
|        |           |         |          |     Code, Item code, Bill |
|        |           |         |          |     Sundry master Code,   |
|        |           |         |          |     Quantity in Main/alt  |
|        |           |         |          |     Unit etc.             |
|        |           |         |          |                           |
|        |           |         |          | 3.  Tran3 -- Reference    |
|        |           |         |          |     Code, Material Centre |
|        |           |         |          |     code, Item Code,      |
|        |           |         |          |     Batch Number, Status, |
|        |           |         |          |     due Date etc.         |
|        |           |         |          |                           |
|        |           |         |          | 4.  VchOtherInfo --       |
|        |           |         |          |     Optional fields,      |
|        |           |         |          |     Transport Details     |
|        |           |         |          |                           |
|        |           |         |          | 5.  Help2 -- Transport    |
|        |           |         |          |     Details               |
|        |           |         |          |                           |
|        |           |         |          | 6.  ItemDesc -- Voucher   |
|        |           |         |          |     Code, Item            |
|        |           |         |          |     Description           |
+--------+-----------+---------+----------+---------------------------+
| Forms  | frmFo     | udtF    | Cvch     | 1.  Tran8 -- Voucher      |
| Re     | rmsRecIss | ormHead | DataForm |     Code/ Type, Form      |
| ceived |           |         |          |     Code/Number, Party    |
|        |           |         |          |     Code etc.             |
|        |           |         |          |                           |
|        |           |         |          | 2.  Tran9 -- Form         |
|        |           |         |          |     Code/Number, Bill     |
|        |           |         |          |     Code, Amount etc.     |
+--------+-----------+---------+----------+---------------------------+
| Forms  | frmFo     | udtF    | Cvch     | 1.  Tran8 -- Voucher      |
| Issued | rmsRecIss | ormHead | DataForm |     Code/Type, Form       |
|        |           |         |          |     Received Date, Form   |
|        |           |         |          |     Number etc.           |
|        |           |         |          |                           |
|        |           |         |          | 2.  Tran9 -- Party Code,  |
|        |           |         |          |     Form Code, Bill Code, |
|        |           |         |          |     amount etc.           |
+--------+-----------+---------+----------+---------------------------+
| VAT    | frmV      | u       |          | 1.  VATInfo -- Date,      |
| J      | ATJournal | dtVATJV |          |     Amount etc.           |
| ournal |           |         |          |                           |
+--------+-----------+---------+----------+---------------------------+
| Adjust | frm       | udtExc  | CEx      | 1.  Excise -- Voucher     |
| Excise | ExciseAdj | iseInfo | ciseInfo |     Code, Date, PLA       |
| A      |           |         |          |     Amount Adjusted etc.  |
| mounts |           |         |          |                           |
+--------+-----------+---------+----------+---------------------------+

After going through the above document, you will have a broad idea of
database structure of BUSY.
