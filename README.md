# Photo archive service

## Concept

Simple wrapper of photo and movies at personal web storage, including 
preview and simple permission for distribution. 

Not targeting:

- integrated administrative user authentication
- online upload (for initial version)
- integration to external authentication like OAuth etc.

## System Requirements

- backend storage
  - stored in hash directory
  - one resource ID per one photo, with multiple resource category identifiers (e.g. JPEG, RAW, thumbnail, web-quality JPEG)
- download
  - download both single with potential multiple category by file or zip, or multiple selection (one category) by zip
  - selection of cateogry
- UI
  - panel like display of thumbnails and full window popup preview
  - selection of multiple thumbnails and operation, like download, creation of sub-album
- access control


## Backend Design

### Administrative interface

- (for initial version) access shall be managed by httpd (etc.)
 
### File storage and distribution

- Physically stored in hash directory
  - unique key as hadh ID
  - cache of thumbnails etc. are stored in the same directory
  - distribution via server script but not directly from storage (loaded by httpd)
- `base photo group` is locked to uploaded group
  - display permission is assigned based on `base photo group`
  - like directory of local storage
  - all properties are stored into database

### Permission

- group based permission, every group can issue multiple `password`s and granted through `permission group`
  - `permission group`, to be used from other groups as simple replace of `password`
  - `base photo group`, to be used for access handling of each photo
  - album group, to be used for access handling of album index page
- `password` is just an access key, not user authentication
  - could be issued or revorked per each `password`

- permissions on accessing to resources
  - album index can be shown based on album group
  - each photo can be shown based on `base photo group`
- clients (not user) are recognized by `cookie`
  - once user inputs `password`, granted permission is stored into database
  - client shall be possible to revoke its `cookie`
- revoke by admin
  - `password` revoke flag (prevent failure to re-use the same `password`)
  - `cookie`, remove all granted permissions on database

### Album

- user can define album as a collection of photoes from multiplebase groups
  - permission TBD

## Database design

## Interface design

Three server interfaces

- static html pages
- album json API: list of albums, album information
- media file distribution

Archive download via zip is not included in the initial version.

