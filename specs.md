
- telegram robot

## expense management

- cmd 'expense'
- robot asks: new, list, delete, edit, ... (the sub commands)
- new:
    - args:
        - see below under workflow mgmt (robot asks this info in userfriendly way)
        - is foto or uploaded file (if already scanned before)
    - info is stored in ipfs
        - png or jpg of foto
        - json message (which has link to png/jpg) for the structure info
    - hash of ipfs is stored in redis hset & also queue so we know we need to process
    - returns: a 4 letter random key (easy to type e.g. a63r, all lowercase digits/nrs, check in redis is unique)
    
### expense workflow management

- each entry in redis for expense has following info
  - key (4 easly digits/letters)
  - foto/file of expense (link to ipfs)
  - company: predefined list of companies (config of robot)
  - tags: tags/labels like used in python jumpscale
  - project: free text      
  - owner (user name from telegram)
  - remark
  - date: default value is current date
  - state
      - value: NEW, ERP, APPROVED, PAID
      - date: is date when above state changes
  
### tool to put in odoo

- a script which is used by an admin person, done per company
- files expenses in odoo, per inserted item go to right page in odoo, allow admin to make changes, set state from NEW to ERP

## archival tool

- robot
    - args:
        - see below (robot asks this info in userfriendly way)
        - is foto or uploaded file (if already scanned before)
    - info is stored in ipfs
        - png or jpg or file
        - json message (which has link to png/jpg) for the structure info
    - hash of ipfs is stored in redis hset (of the json message)
    - returns: 
        - a url with at end 5 letter random key (easy to type e.g. wa63r, all lowercase digits/nrs, check in redis is unique)
        - e.g. http://docs.greenitglobe.com/wa63r.jpg
        - this can then be copied to e.g. an erp system

- each entry in redis for archival has following info
  - key (5 easly digits/letters)
  - foto/file of doc (link to ipfs)
  - company: predefined list of companies (config of robot)
  - category: predefined list of categories
  - tags: tags/labels like used in python jumpscale
  - owner (user name from telegram)
  - remark
  - insertDate: default value is current date
  - lastUsedDate: last time downloaded
  - state
      - value: NEW, ACTIVE, ARCHIVED
      - date: is date when above state changes
      
- retrieval tool
    - webserver which exposes the info e.g. http://docs.greenitglobe.com/wa63r.jpg
  
