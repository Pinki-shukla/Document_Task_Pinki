# Ldap documentation for finoptaplus

## Task requirement:
- To setup ldap for the fino ptaplus project.

## Environment Details:
- OS Version: CentOS Stream 8
- Podman Version: 4.6.1
- Java Version: JDK 11.0.18 (LTS)
- Apache Directory Version: 2.0.0-M17
## List of Tools and Technologies:
- Podman
- OpenLDAP
- Apache Directory Studio
- Java (JDK)

# Step 1: Setup 389 Directory Server on Podman
- <b>1.1. Create a Directory:</b>
  
Create a directory to store configuration and data. You can do this with the command:
```
mkdir 389ds
```


mkdir is used to make the directory 

- <b>1.2. Run 389 Directory Server in Podman:</b>
Start a 389 Directory Server container using Podman with the following command:

```
podman run -dt --name 389ds-ldap -p 3389:3389  -v ~/389ds/:/data:Z -e DS_SUFFIX=dc=finoptaplus,dc=com -e DS_DM_PASSWORD=pinkishukla docker.io/389ds/dirsrv
```
![Alt text](/images/ldap%20run%20container%201,2.png)


This command sets up a container named 389ds-ldap with the specified configuration.

- <b>1.3. Install LDAP Clients on Bash Machine:</b>
On your CentOS machine, install LDAP utilities using the following command:

(On CentOS or CentOS Stream, you typically use the package openldap-clients to get the LDAP utilities. If you need LDAP utilities, you can install openldap-clients.)

```
sudo yum install openldap-clients
```
![Alt text](/images/ldap%20utils.png)

# Step 2: Check and Create Suffix
 if suffix not create then create with this command

- <b>2.1. Check if the Suffix is Created:</b>
Access the container using the following command:

```
podman exec -it 389ds-ldap bash
```

Check if the suffix exists by running:

```
dsconf -D "cn=Directory Manager" ldap://localhost:3389 backend suffix list
```
![Alt text](./images/check%20backends.png)

- <b>2.2. Create Suffix (If Not Exist):</b>
  
 If the suffix doesn't exist, you can create it with this command:

 ```
 dsconf -D "cn=Directory Manager" ldap://localhost:3389 backend create --suffix "dc=finoptaplus,dc=com" --be-name "finoptaplus"
```
![Alt text](./images/3backends%20create.png)

# Step 3: Adding LDIF Files

- errorCode.ldif
- businessTypes.ldif
- businesstypes_code.ldif
- glsfinal.ldif
- banks.ldif
- accounts.ldif
- commonAttributes.ldif
- objectClasses.ldif
- mastersForErrorHoldRestrictions.ldif
- merchants.ldif
- partners.ldif
- ptapluserror_code.ldif
- status.ldif
- states.ldif
- transactionGroupsAndTransactionTypes.ldif
- directory.ldif
- Admin5.ldif

- <b>3.1. Create a Directory for LDIF Files:</b>
  
Create a directory named "ldifs_files" to store the LDIF files.
```
mkdir ldifs_files
```
Now, go inside the directory name "ldifs_files" by using following command.

```
cd ldifs_files
```
And now create all files one by one by using vim command and add data .

```
vim errorCode.ldif
```

![Alt text](/images/add%20error.ldif%20file.png)


```
Vim errorCode.ldif 
```

- <b>Add data in errorCode.ldif file</b>

```
dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( PTAErrorcode-oid NAME 'PTAErrorcode' DESC 'PTAErrorcode, Integer' EQUALITY integerMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( ErrorSource-oid NAME 'ErrorSource' DESC 'ErrorSource, stringIA5' EQUALITY caseIgnoreIA5Match SUBSTR caseIgnoreIA5SubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( NativeErrorCode-oid NAME 'NativeErrorCode' DESC 'NativeErrorCode, Integer' EQUALITY integerMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( NativeErrorMessage-oid NAME 'NativeErrorMessage' DESC 'NativeErrorMessage, stringIA5' EQUALITY caseIgnoreIA5Match SUBSTR caseIgnoreIA5SubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( NativeErrorDescription-oid NAME 'NativeErrorDescription' DESC 'NativeErrorDescription, stringIA5' EQUALITY caseIgnoreIA5Match SUBSTR caseIgnoreIA5SubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( HTTPCode-oid NAME 'HTTPCode' DESC 'HTTPCode, Integer' EQUALITY integerMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( StatusCode-oid NAME 'StatusCode' DESC 'StatusCode, Integer' EQUALITY integerMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )


dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( HTTPCodeDescription-oid NAME 'HTTPCodeDescription' DESC 'HTTPCodeDescription, stringIA5' EQUALITY caseIgnoreIA5Match SUBSTR caseIgnoreIA5SubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( ErrorMessage-oid NAME 'ErrorMessage' DESC 'ErrorMessage, stringIA5' EQUALITY caseIgnoreIA5Match SUBSTR caseIgnoreIA5SubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( TroubleshootMessage-oid NAME 'TroubleshootMessage' DESC 'TroubleshootMessage, stringIA5' EQUALITY caseIgnoreIA5Match SUBSTR caseIgnoreIA5SubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )


dn: cn=schema
changetype: modify
add: objectclasses
objectclasses: ( errorcodeptaplus-oid NAME 'errorcodeptaplus' SUP top STRUCTURAL MUST (ErrorSource $ PTAErrorcode $ NativeErrorCode $ HTTPCode $ StatusCode $ ErrorMessage ) MAY (TroubleshootMessage $ HTTPCodeDescription $ NativeErrorMessage $ NativeErrorDescription) X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )
```
- <b>Add data in business.ldif file</b>
```
vim businessTypes.ldif
```
```
#business type

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( mCategory-oid NAME 'mCategory' DESC 'mCategory,string' EQUALITY caseIgnoreIA5Match SUBSTR caseIgnoreIA5SubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( mCode-oid NAME 'mCode' DESC 'mCode,String' EQUALITY caseIgnoreIA5Match SUBSTR caseIgnoreIA5SubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: attributetypes
attributetypes: ( businessTypeUid-oid NAME 'businessTypeUid' DESC 'integer' EQUALITY integerMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 SINGLE-VALUE X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=schema
changetype: modify
add: objectclasses
objectclasses: ( businessTypes-oid NAME 'businessTypes' SUP top STRUCTURAL MUST (businessTypeUid $ mCode) MAY (mCategory $ member ) X-ORIGIN ( 'Fino PtaPlus' 'user defined' ) )

dn: cn=businessTypeUid,cn=Distributed Numeric Assignment Plugin,cn=plugins,cn=config
objectClass: top
objectClass: extensibleObject
cn: businessTypeUid
dnatype: businessTypeUid
dnafilter: (objectclass=businessTypes)
dnascope: dc=FinoPtaPlus,dc=com
dnanextvalue: 1

```

- <b>Add data in businesstypes_code.ldif file</b>

```
vim businesstypes_code.ldif
```

```
version: 1

dn: ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: organizationalUnit
objectClass: top
ou: businessTypes
description:: YnVzaW5lc3MgdHlwZSBkZXRhaWxzICA=
aci: (targetattr = "*")(version 3.0; aci "read"; allow(read,search,compare) 
 groupdn="ldap:///cn=partneradmin,ou=roles,dc=finoptaplus,dc=com  || ldap://
 /cn=backoffice,ou=roles,dc=finoptaplus,dc=com";)

dn: mcode=763,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 276
mCode: 763
mCategory: Agricultural co-operatives

dn: mcode=780,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 277
mCode: 780
mCategory: Landscaping and horticultural services

dn: mcode=5995,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 278
mCode: 5995
mCategory: Pet shops, pet food and supplies

dn: mcode=5611,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 279
mCode: 5611
mCategory: Men's and boys' clothing and accessory shops

dn: mcode=5621,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 280
mCode: 5621
mCategory: Women's ready-to-wear shops

dn: mcode=5631,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 281
mCode: 5631
mCategory: Women's accessory and speciality shops

dn: mcode=5641,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 282
mCode: 5641
mCategory: Children's and infants' wear shops

dn: mcode=5651,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 283
mCode: 5651
mCategory: Family clothing shops

dn: mcode=5661,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 284
mCode: 5661
mCategory: Shoe shops

dn: mcode=5681,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 285
mCode: 5681
mCategory: Furriers and fur shops

dn: mcode=5691,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 286
mCode: 5691
mCategory: Men's and women's clothing shops

dn: mcode=5697,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 287
mCode: 5697
mCategory: Tailors, seamstresses, mending and alterations

dn: mcode=5698,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 288
mCode: 5698
mCategory: Wig and toupee shops

dn: mcode=5699,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 289
mCode: 5699
mCategory: Miscellaneous apparel and accessory shops

dn: mcode=7210,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 290
mCode: 7210
mCategory: Laundry, cleaning and garment services

dn: mcode=7211,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 291
mCode: 7211
mCategory: Laundry services - family and commercial

dn: mcode=7216,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 292
mCode: 7216
mCategory: Dry cleaners

dn: mcode=7217,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 293
mCode: 7217
mCategory: Carpet and upholstery cleaning

dn: mcode=7296,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 294
mCode: 7296
mCategory: Clothing rentals - costumes, uniforms and formal wear

dn: mcode=1520,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 295
mCode: 1520
mCategory: General contractors - residential and commercial

dn: mcode=1711,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 296
mCode: 1711
mCategory: Heating, plumbing and air-conditioning contractors

dn: mcode=1731,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 297
mCode: 1731
mCategory: Electrical contractors

dn: mcode=1740,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 298
mCode: 1740
mCategory: Masonry, stonework, tile setting, plastering and insulation contr
 actors

dn: mcode=1750,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 299
mCode: 1750
mCategory: Carpentry contractors

dn: mcode=1761,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 300
mCode: 1761
mCategory: Roofing, siding and sheet metal work contractors

dn: mcode=1771,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 301
mCode: 1771
mCategory: Concrete work contractors

dn: mcode=1799,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 302
mCode: 1799
mCategory: Special trade contractors - not elsewhere classified

dn: mcode=2741,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 303
mCode: 2741
mCategory: Miscellaneous publishing and printing services

dn: mcode=2791,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 304
mCode: 2791
mCategory: Typesetting, platemaking and related services

dn: mcode=2842,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 305
mCode: 2842
mCategory: Speciality cleaning, polishing and sanitation preparations

dn: mcode=5942,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 306
mCode: 5942
mCategory: Bookshops

dn: mcode=5943,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 307
mCode: 5943
mCategory: Stationery, office and school supply shops

dn: mcode=8211,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 308
mCode: 8211
mCategory: Elementary and secondary schools

dn: mcode=8220,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 309
mCode: 8220
mCategory: Colleges, universities, professional schools and junior colleges

dn: mcode=8241,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 310
mCode: 8241
mCategory: Correspondence schools

dn: mcode=8244,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 311
mCode: 8244
mCategory: Business and secretarial schools

dn: mcode=8249,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 312
mCode: 8249
mCategory: Trade and vocational schools

dn: mcode=8299,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 313
mCode: 8299
mCategory: Schools and educational services - not elsewhere classified

dn: mcode=5722,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 314
mCode: 5722
mCategory: Household appliance shops

dn: mcode=5732,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 315
mCode: 5732
mCategory: Electronics shops

dn: mcode=5733,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 316
mCode: 5733
mCategory: Music shops - musical instruments, pianos and sheet music

dn: mcode=5734,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 317
mCode: 5734
mCategory: Computer software outlets

dn: mcode=5735,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 318
mCode: 5735
mCategory: Record shops

dn: mcode=5815,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 319
mCode: 5815
mCategory: Digital Goods: Media, Books, Movies, Music

dn: mcode=5816,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 320
mCode: 5816
mCategory: Digital Goods: Games

dn: mcode=5817,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 321
mCode: 5817
mCategory: Digital Goods: Applications (Excludes Games)

dn: mcode=5818,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 322
mCode: 5818
mCategory: Digital Goods: Large Digital Goods Merchant

dn: mcode=5946,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 323
mCode: 5946
mCategory: Camera and photographic supply shops

dn: mcode=7221,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 324
mCode: 7221
mCategory: Photographic studios

dn: mcode=7372,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 325
mCode: 7372
mCategory: Computer programming, data processing and integrated systems desi
 gn services

dn: mcode=7379,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 326
mCode: 7379
mCategory: Computer maintenance and repair services - not elsewhere classifi
 ed

dn: mcode=7622,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 327
mCode: 7622
mCategory: Electronics repair shops

dn: mcode=7623,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 328
mCode: 7623
mCategory: Air conditioning and refrigeration repair shops

dn: mcode=7629,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 329
mCode: 7629
mCategory: Electrical and small appliance repair shops

dn: mcode=7631,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 330
mCode: 7631
mCategory: Watch, clock and jewellery repair shops

dn: mcode=7829,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 331
mCode: 7829
mCategory: Motion picture and video tape production and distribution

dn: mcode=7832,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 332
mCode: 7832
mCategory: Motion picture theatres

dn: mcode=7911,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 333
mCode: 7911
mCategory: Dance halls, studios and schools

dn: mcode=7922,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 334
mCode: 7922
mCategory: Theatrical producers (except motion pictures) and ticket agencies

dn: mcode=7929,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 335
mCode: 7929
mCategory: Bands, orchestras and miscellaneous entertainers - not elsewhere 
 classified

dn: mcode=7932,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 336
mCode: 7932
mCategory: Billiard and pool establishments

dn: mcode=7933,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 337
mCode: 7933
mCategory: Bowling alleys

dn: mcode=7991,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 338
mCode: 7991
mCategory: Tourist attractions and exhibits

dn: mcode=7993,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 339
mCode: 7993
mCategory: Video amusement game supplies

dn: mcode=7994,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 340
mCode: 7994
mCategory: Video game arcades and establishments

dn: mcode=7995,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 341
mCode: 7995
mCategory: Betting, including lottery tickets, casino gaming chips, off-trac
 k betting and wagers at race tracks

dn: mcode=7996,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 342
mCode: 7996
mCategory: Amusement parks, circuses, carnivals and fortune tellers

dn: mcode=7998,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 343
mCode: 7998
mCategory: Aquariums, seaquariums and dolphinariums

dn: mcode=7999,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 344
mCode: 7999
mCategory: Recreation services - not elsewhere classified

dn: mcode=6010,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 345
mCode: 6010
mCategory: Financial institutions manual cash disbursements/Cash Withdrawal

dn: mcode=6011,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 346
mCode: 6011
mCategory: Financial institutions - automated cash disbursements

dn: mcode=6051,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 347
mCode: 6051
mCategory: Non-financial institutions - foreign currency, money orders (not 
 wire transfer), scrip and travellers' checks

dn: mcode=6540,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 348
mCode: 6540
mCategory: Debit card to wallet credit (Wallet top up)

dn: mcode=7321,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 349
mCode: 7321
mCategory: Consumer credit reporting agencies

dn: mcode=5811,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 350
mCode: 5811
mCategory: Caterers

dn: mcode=5812,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 351
mCode: 5812
mCategory: Eating places and restaurants

dn: mcode=5814,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 352
mCode: 5814
mCategory: Fast food restaurants

dn: mcode=9223,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 353
mCode: 9223
mCategory: Bail and bond payments

dn: mcode=9402,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 354
mCode: 9402
mCategory: Postal services - government only

dn: mcode=8111,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 355
mCode: 8111
mCategory: Legal services and attorneys

dn: mcode=8398,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 356
mCode: 8398
mCategory: Charitable and social service organizations - PM Relief Fund

dn: mcode=8641,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 357
mCode: 8641
mCategory: Civic, social and fraternal associations

dn: mcode=8651,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 358
mCode: 8651
mCategory: Political organizations

dn: mcode=7230,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 359
mCode: 7230
mCategory: Beauty and barber shops

dn: mcode=7273,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 360
mCode: 7273
mCategory: Dating and escort services

dn: mcode=7297,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 361
mCode: 7297
mCategory: Massage parlours

dn: mcode=7298,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 362
mCode: 7298
mCategory: Health and beauty spas

dn: mcode=7299,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 363
mCode: 7299
mCategory: Miscellaneous personal services - not elsewhere classified

dn: mcode=5962,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 364
mCode: 5962
mCategory: Telemarketing - travel-related arrangement services

dn: mcode=5963,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 365
mCode: 5963
mCategory: Door-to-door sales

dn: mcode=5964,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 366
mCode: 5964
mCategory: Direct marketing - catalogue merchants

dn: mcode=5965,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 367
mCode: 5965
mCategory: Direct marketing - combination catalogue and retail merchants

dn: mcode=5966,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 368
mCode: 5966
mCategory: Direct marketing - outbound telemarketing merchants

dn: mcode=5967,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 369
mCode: 5967
mCategory: Direct marketing - inbound telemarketing merchants

dn: mcode=5968,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 370
mCode: 5968
mCategory: Direct marketing - continuity/subscription merchants

dn: mcode=7311,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 371
mCode: 7311
mCategory: Advertising services

dn: mcode=7333,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 372
mCode: 7333
mCategory: Commercial photography, art and graphics

dn: mcode=5912,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 373
mCode: 5912
mCategory: Drug stores and pharmacies

dn: mcode=5975,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 374
mCode: 5975
mCategory: Hearing aids - sales, service and supplies

dn: mcode=5976,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 375
mCode: 5976
mCategory: Orthopaedic goods and prosthetic devices

dn: mcode=7339,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 376
mCode: 7339
mCategory: Stenographic and secretarial support services

dn: mcode=8011,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 377
mCode: 8011
mCategory: Doctors and physicians - not elsewhere classified

dn: mcode=8021,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 378
mCode: 8021
mCategory: Dentists and orthodontists

dn: mcode=8031,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 379
mCode: 8031
mCategory: Osteopaths

dn: mcode=8041,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 380
mCode: 8041
mCategory: Chiropractors

dn: mcode=8042,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 381
mCode: 8042
mCategory: Optometrists and ophthalmologists

dn: mcode=8043,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 382
mCode: 8043
mCategory: Opticians, optical goods and eyeglasses

dn: mcode=8049,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 383
mCode: 8049
mCategory: Podiatrists and chiropodists

dn: mcode=8050,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 384
mCode: 8050
mCategory: Nursing and personal care facilities

dn: mcode=8062,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 385
mCode: 8062
mCategory: Hospitals

dn: mcode=8071,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 386
mCode: 8071
mCategory: Medical and dental laboratories

dn: mcode=8099,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 387
mCode: 8099
mCategory: Medical services and health practitioners not elsewhere classifie
 d

dn: mcode=5935,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 388
mCode: 5935
mCategory: Wrecking and salvage yards

dn: mcode=5994,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 389
mCode: 5994
mCategory: Newsagents and news-stands

dn: mcode=7261,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 390
mCode: 7261
mCategory: Funeral services and crematoriums

dn: mcode=7277,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 391
mCode: 7277
mCategory: Counselling services - debt, marriage and personal

dn: mcode=7342,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 392
mCode: 7342
mCategory: Exterminating and disinfecting services

dn: mcode=7349,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 393
mCode: 7349
mCategory: Cleaning, maintenance and janitorial services

dn: mcode=7361,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 394
mCode: 7361
mCategory: Employment agencies and temporary help services

dn: mcode=7375,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 395
mCode: 7375
mCategory: Information retrieval services

dn: mcode=7392,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 396
mCode: 7392
mCategory: Management, consulting and public relations services

dn: mcode=7393,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 397
mCode: 7393
mCategory: Detective agencies, protective agencies and security services, in
 cluding armoured cars and guard dogs

dn: mcode=7395,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 398
mCode: 7395
mCategory: Photofinishing laboratories and photo developing

dn: mcode=7399,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 399
mCode: 7399
mCategory: Business services - not elsewhere classified

dn: mcode=8351,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 400
mCode: 8351
mCategory: Child care services

dn: mcode=8661,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 401
mCode: 8661
mCategory: Religious organizations

dn: mcode=8699,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 402
mCode: 8699
mCategory: Membership organization - not elsewhere classified

dn: mcode=8734,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 403
mCode: 8734
mCategory: Testing laboratories (non-medical)

dn: mcode=8911,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 404
mCode: 8911
mCategory: Architectural, engineering and surveying services

dn: mcode=8999,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 405
mCode: 8999
mCategory: Professional services - not elsewhere classified

dn: mcode=6513,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 406
mCode: 6513
mCategory: Real Estate Agents and Managers - Rentals

dn: mcode=7011,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 407
mCode: 7011
mCategory: Lodging - hotels, motels and resorts

dn: mcode=5013,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 408
mCode: 5013
mCategory: Motor vehicle supplies and new parts

dn: mcode=5021,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 409
mCode: 5021
mCategory: Office and commercial furniture

dn: mcode=5039,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 410
mCode: 5039
mCategory: Construction materials - not elsewhere classified

dn: mcode=5044,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 411
mCode: 5044
mCategory: Office, photographic, photocopy and microfilm equipment

dn: mcode=5045,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 412
mCode: 5045
mCategory: Computers, computer peripheral equipment - not elsewhere classifi
 ed

dn: mcode=5046,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 413
mCode: 5046
mCategory: Commercial equipment - not elsewhere classified

dn: mcode=5047,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 414
mCode: 5047
mCategory: Dental/laboratory/medical/ophthalmic hospital equipment and suppl
 ies

dn: mcode=5051,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 415
mCode: 5051
mCategory: Metal service centres and offices

dn: mcode=5065,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 416
mCode: 5065
mCategory: Electrical parts and equipment

dn: mcode=5072,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 417
mCode: 5072
mCategory: Hardware equipment and supplies

dn: mcode=5074,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 418
mCode: 5074
mCategory: Plumbing and heating equipment and supplies

dn: mcode=5085,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 419
mCode: 5085
mCategory: Industrial supplies - not elsewhere classified

dn: mcode=5094,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 420
mCode: 5094
mCategory: Precious stones and metals, watches and jewellery

dn: mcode=5099,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 421
mCode: 5099
mCategory: Durable goods - not elsewhere classified

dn: mcode=5111,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 422
mCode: 5111
mCategory: Stationery, office supplies and printing and writing paper

dn: mcode=5122,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 423
mCode: 5122
mCategory: Drugs, drug proprietors

dn: mcode=5131,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 424
mCode: 5131
mCategory: Piece goods, notions and other dry goods

dn: mcode=5137,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 425
mCode: 5137
mCategory: Men's, women's and children's uniforms and commercial clothing

dn: mcode=5139,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 426
mCode: 5139
mCategory: Commercial footwear

dn: mcode=5169,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 427
mCode: 5169
mCategory: Chemicals and allied products - not elsewhere classified

dn: mcode=5192,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 428
mCode: 5192
mCategory: Books, periodicals and newspapers

dn: mcode=5193,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 429
mCode: 5193
mCategory: Florists' supplies, nursery stock and flowers

dn: mcode=5198,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 430
mCode: 5198
mCategory: Paints, varnishes and supplies

dn: mcode=5199,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 431
mCode: 5199
mCategory: Non-durable goods - not elsewhere classified

dn: mcode=5200,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 432
mCode: 5200
mCategory: Home supply warehouse outlets

dn: mcode=5211,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 433
mCode: 5211
mCategory: Lumber and building materials outlets

dn: mcode=5231,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 434
mCode: 5231
mCategory: Glass, paint and wallpaper shops

dn: mcode=5251,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 435
mCode: 5251
mCategory: Hardware shops

dn: mcode=5261,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 436
mCode: 5261
mCategory: Lawn and garden supply outlets, including nurseries

dn: mcode=5271,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 437
mCode: 5271
mCategory: Mobile home dealers

dn: mcode=5300,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 438
mCode: 5300
mCategory: Wholesale clubs

dn: mcode=5309,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 439
mCode: 5309
mCategory: Duty-free shops

dn: mcode=5310,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 440
mCode: 5310
mCategory: Discount shops

dn: mcode=5311,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 441
mCode: 5311
mCategory: Department stores

dn: mcode=5331,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 442
mCode: 5331
mCategory: Variety stores

dn: mcode=5399,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 443
mCode: 5399
mCategory: Miscellaneous general merchandise

dn: mcode=5411,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 444
mCode: 5411
mCategory: Groceries and supermarkets

dn: mcode=5422,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 445
mCode: 5422
mCategory: Freezer and locker meat provisioners

dn: mcode=5441,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 446
mCode: 5441
mCategory: Candy, nut and confectionery shops

dn: mcode=5451,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 447
mCode: 5451
mCategory: Dairies

dn: mcode=5462,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 448
mCode: 5462
mCategory: Bakeries

dn: mcode=5499,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 449
mCode: 5499
mCategory: Miscellaneous food shops - convenience and speciality retail outl
 ets

dn: mcode=5511,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 450
mCode: 5511
mCategory: Car and truck dealers (new and used) sales, services, repairs, pa
 rts and leasing

dn: mcode=5521,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 451
mCode: 5521
mCategory: Car and truck dealers (used only) sales, service, repairs, parts 
 and leasing

dn: mcode=5531,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 452
mCode: 5531
mCategory: Auto and home supply outlets

dn: mcode=5532,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 453
mCode: 5532
mCategory: Automotive tyre outlets

dn: mcode=5533,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 454
mCode: 5533
mCategory: Automotive parts and accessories outlets

dn: mcode=5541,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 455
mCode: 5541
mCategory: Service stations (with or without ancillary services)

dn: mcode=5551,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 456
mCode: 5551
mCategory: Boat dealers

dn: mcode=5561,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 457
mCode: 5561
mCategory: Camper, recreational and utility trailer dealers

dn: mcode=5571,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 458
mCode: 5571
mCategory: Motorcycle shops and dealers

dn: mcode=5592,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 459
mCode: 5592
mCategory: Motor home dealers

dn: mcode=5598,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 460
mCode: 5598
mCategory: Snowmobile dealers

dn: mcode=5599,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 461
mCode: 5599
mCategory: Miscellaneous automotive, aircraft and farm equipment dealers - n
 ot elsewhere classified

dn: mcode=5712,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 462
mCode: 5712
mCategory: Furniture, home furnishings and equipment shops and manufacturers
 , except appliances

dn: mcode=5713,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 463
mCode: 5713
mCategory: Floor covering services

dn: mcode=5714,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 464
mCode: 5714
mCategory: Drapery, window covering and upholstery shops

dn: mcode=5718,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 465
mCode: 5718
mCategory: Fireplaces, fireplace screens and accessories shops

dn: mcode=5719,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 466
mCode: 5719
mCategory: Miscellaneous home furnishing speciality shops

dn: mcode=5931,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 467
mCode: 5931
mCategory: Used merchandise and second-hand shops

dn: mcode=5932,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 468
mCode: 5932
mCategory: Antique shops - sales, repairs and restoration services

dn: mcode=5933,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 469
mCode: 5933
mCategory: Pawn shops

dn: mcode=5937,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 470
mCode: 5937
mCategory: Antique reproduction shops

dn: mcode=5944,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 471
mCode: 5944
mCategory: Jewellery, watch, clock and silverware shops

dn: mcode=5945,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 472
mCode: 5945
mCategory: Hobby, toy and game shops

dn: mcode=5947,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 473
mCode: 5947
mCategory: Gift, card, novelty and souvenir shops

dn: mcode=5948,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 474
mCode: 5948
mCategory: Luggage and leather goods shops

dn: mcode=5949,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 475
mCode: 5949
mCategory: Sewing, needlework, fabric and piece goods shops

dn: mcode=5950,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 476
mCode: 5950
mCategory: Glassware and crystal shops

dn: mcode=5970,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 477
mCode: 5970
mCategory: Artist supply and craft shops

dn: mcode=5971,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 478
mCode: 5971
mCategory: Art dealers and galleries

dn: mcode=5972,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 479
mCode: 5972
mCategory: Stamp and coin shops

dn: mcode=5973,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 480
mCode: 5973
mCategory: Religious goods and shops

dn: mcode=5977,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 481
mCode: 5977
mCategory: Cosmetic Stores

dn: mcode=5978,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 482
mCode: 5978
mCategory: Typewriter outlets - sales, service and rentals

dn: mcode=5992,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 483
mCode: 5992
mCategory: Florists

dn: mcode=5993,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 484
mCode: 5993
mCategory: Cigar shops and stands

dn: mcode=5997,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 485
mCode: 5997
mCategory: Electric razor outlets - sales and service

dn: mcode=5998,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 486
mCode: 5998
mCategory: Tent and awning shops

dn: mcode=5999,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 487
mCode: 5999
mCategory: Miscellaneous and speciality retail outlets

dn: mcode=7251,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 488
mCode: 7251
mCategory: Shoe repair shops, shoe shine parlours and hat cleaning shops

dn: mcode=7278,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 489
mCode: 7278
mCategory: Buying and shopping services and clubs

dn: mcode=7332,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 490
mCode: 7332
mCategory: Blueprinting and Photocopying Services

dn: mcode=7338,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 491
mCode: 7338
mCategory: Quick copy, reproduction and blueprinting services

dn: mcode=7394,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 492
mCode: 7394
mCategory: Equipment, tool, furniture and appliance rentals and leasing

dn: mcode=7641,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 493
mCode: 7641
mCategory: Furniture reupholstery, repair and refinishing

dn: mcode=7692,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 494
mCode: 7692
mCategory: Welding services

dn: mcode=7699,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 495
mCode: 7699
mCategory: Miscellaneous repair shops and related services

dn: mcode=7841,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 496
mCode: 7841
mCategory: Video tape rentals

dn: mcode=5715,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 497
mCode: 5715
mCategory: Alcoholic beverage wholesalers

dn: mcode=5813,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 498
mCode: 5813
mCategory: Drinking places (alcoholic beverages) - bars, taverns, night-club
 s, cocktail lounges and discotheques

dn: mcode=5921,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 499
mCode: 5921
mCategory: Package shops - beer, wine and liquor

dn: mcode=5655,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 500
mCode: 5655
mCategory: Sports and riding apparel shops

dn: mcode=5940,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 501
mCode: 5940
mCategory: Bicycle shops - sales and service

dn: mcode=5941,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 502
mCode: 5941
mCategory: Sporting goods shops

dn: mcode=5996,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 503
mCode: 5996
mCategory: Swimming pools - sales, supplies and services

dn: mcode=7012,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 504
mCode: 7012
mCategory: Timeshares

dn: mcode=7032,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 505
mCode: 7032
mCategory: Sporting and recreational camps

dn: mcode=7033,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 506
mCode: 7033
mCategory: Trailer parks and camp-sites

dn: mcode=7941,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 507
mCode: 7941
mCategory: Commercial sports, professional sports clubs, athletic fields and
  sports promoters

dn: mcode=7992,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 508
mCode: 7992
mCategory: Public golf courses

dn: mcode=7997,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 509
mCode: 7997
mCategory: Membership clubs (sports, recreation, athletic), country clubs an
 d private golf courses

dn: mcode=7276,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 510
mCode: 7276
mCategory: Tax preparation services

dn: mcode=8931,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 511
mCode: 8931
mCategory: Accounting, auditing and bookkeeping services

dn: mcode=3020,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 512
mCode: 3020
mCategory: AIR-INDIA

dn: mcode=4011,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 513
mCode: 4011
mCategory: Railroads

dn: mcode=4119,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 514
mCode: 4119
mCategory: Ambulance services

dn: mcode=4121,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 515
mCode: 4121
mCategory: Taxi-cabs and limousines

dn: mcode=4131,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 516
mCode: 4131
mCategory: Bus lines

dn: mcode=4214,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 517
mCode: 4214
mCategory: Motor freight carriers and trucking - local and long distance, mo
 ving and storage companies and local delivery

dn: mcode=4215,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 518
mCode: 4215
mCategory: Courier services - air and ground and freight forwarders

dn: mcode=4225,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 519
mCode: 4225
mCategory: Public warehousing and storage - farm products, refrigerated good
 s and household goods

dn: mcode=4411,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 520
mCode: 4411
mCategory: Steamships and cruise lines

dn: mcode=4457,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 521
mCode: 4457
mCategory: Boat rentals and leasing

dn: mcode=4468,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 522
mCode: 4468
mCategory: Marinas, marine service and supplies

dn: mcode=4511,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 523
mCode: 4511
mCategory: Airlines and air carriers

dn: mcode=4582,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 524
mCode: 4582
mCategory: Airports, flying fields and airport terminals

dn: mcode=4722,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 525
mCode: 4722
mCategory: Travel agencies and tour operators

dn: mcode=4784,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 526
mCode: 4784
mCategory: Tolls and bridge fees

dn: mcode=4789,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 527
mCode: 4789
mCategory: Transportation services - not elsewhere classified

dn: mcode=7511,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 528
mCode: 7511
mCategory: Truck Stop

dn: mcode=7512,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 529
mCode: 7512
mCategory: Automobile rentals

dn: mcode=7513,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 530
mCode: 7513
mCategory: Truck and utility trailer rentals

dn: mcode=7519,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 531
mCode: 7519
mCategory: Motor home and recreational vehicle rentals

dn: mcode=7523,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 532
mCode: 7523
mCategory: Parking lots and garages

dn: mcode=7531,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 533
mCode: 7531
mCategory: Automotive body repair shops

dn: mcode=7534,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 534
mCode: 7534
mCategory: Tyre retreading and repair shops

dn: mcode=7535,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 535
mCode: 7535
mCategory: Automotive paint shops

dn: mcode=7538,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 536
mCode: 7538
mCategory: Automotive service shops (non-dealer)

dn: mcode=7542,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 537
mCode: 7542
mCategory: Car washes

dn: mcode=7549,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 538
mCode: 7549
mCategory: Towing services

dn: mcode=8675,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 539
mCode: 8675
mCategory: Automobile associations

dn: mcode=4812,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 540
mCode: 4812
mCategory: Telecommunication equipment and telephone sales

dn: mcode=4814,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 541
mCode: 4814
mCategory: Telecommunication services, including local and long distance cal
 ls, credit card calls, calls through use of magnetic stripe reading telepho
 nes and faxes

dn: mcode=4815,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 542
mCode: 4815
mCategory: Monthly summary telephone charges

dn: mcode=4816,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 543
mCode: 4816
mCategory: Computer network/information services

dn: mcode=4821,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 544
mCode: 4821
mCategory: Telegraph services

dn: mcode=4829,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 545
mCode: 4829
mCategory: Wire transfers and money orders

dn: mcode=4899,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 546
mCode: 4899
mCategory: Cable and other pay television services

dn: mcode=4900,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 547
mCode: 4900
mCategory: Utilities - electric, gas, water and sanitary

dn: mcode=742,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 548
mCode: 742
mCategory: Veterinary services

dn: mcode=743,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 549
mCode: 743
mCategory: Wine producers

dn: mcode=744,ou=businessTypes,dc=FinoPtaPlus,dc=com
objectClass: businessTypes
objectClass: top
businessTypeUid: 550
mCode: 744
mCategory: Champagne producers

```



# Step 4 : Ldif file add command
```
ldapadd -a -c -xH ldap://localhost:3389 -D "cn=Directory Manager" -W -f errorCode.ldif

```
The command is used to add data to an LDAP directory by connecting to the LDAP server at ldap://localhost:3389



![Alt text](/images/ldap%20add.png)

# Step 5 : Install Apache Directory Studio :-
 https://directory.apache.org/studio/download/download-linux.html by using this link.

- <b>Note :- To use apache Directory studio you need to install JDK.

![Alt text](/images/apache%20directory%20studio%20installed.png)

# Step 6 : Install JDK by using this command:-

```
sudo dnf install java-11-openjdk
```
Check version by using this command

```
java -version
```