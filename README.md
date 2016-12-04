# React Native Paged Contacts

Paged contacts manager for React Native.

**Currently, only fetching contacts is supported.**

##API

- `new PagedContacts()` — Create a paged contacts manager for all device contacts.
- `new PagedContacts(nameMatch)` — Create a paged contacts manager for contacts matching the provided name.
- `requestAccess()` — Request contacts access from the operating system. This must be called before calling other APIs.
- `setNameMatch(matchName)` — Change the result set to filter contacts by matching name. Set to `null` to receive all contacts.
- `getContactsCount()` — Get the count of the current contacts set.
- `getContactsWithRange(offset, batchSize, keysToFetch)` — Get contacts within the requested `batchSize`, starting from `offset`. Only the keys requested in `keysToFetch` will be provided (contact identifiers are always provided).
- `getContactsWithIdentifiers(identifiers, keysToFetch)` — Get contacts with the provided `identifiers`. Only the keys requested in `keysToFetch` will be provided (contact identifiers are always provided).
- `dispose()` — Disposes the native components. Call this method when the manager object is no longer required. Must not call any other methods of the contacts manager after calling `dispose`.

####Available Keys to Fetch

- `PagedContacts.identifier`
- `PagedContacts.displayName`
- `PagedContacts.namePrefix`
- `PagedContacts.givenName`
- `PagedContacts.middleName`
- `PagedContacts.familyName`
- `PagedContacts.previousFamilyName`
- `PagedContacts.nameSuffix`
- `PagedContacts.nickname`
- `PagedContacts.organizationName`
- `PagedContacts.departmentName`
- `PagedContacts.jobTitle`
- `PagedContacts.phoneticGivenName`
- `PagedContacts.phoneticMiddleName`
- `PagedContacts.phoneticFamilyName`
- `PagedContacts.phoneticOrganizationName`
- `PagedContacts.birthday`
- `PagedContacts.nonGregorianBirthday`
- `PagedContacts.note`
- `PagedContacts.imageData`
- `PagedContacts.thumbnailImageData`
- `PagedContacts.phoneNumbers`
- `PagedContacts.emailAddresses`
- `PagedContacts.postalAddresses`
- `PagedContacts.dates`
- `PagedContacts.urlAddresses`
- `PagedContacts.relations`
- `PagedContacts.socialProfiles`
- `PagedContacts.instantMessageAddresses`

##Usage

Import the library and create a new `PagedContacts` instance.

```javascript
import {PagedContacts} from 'react-native-paged-contacts';
let pg = new PagedContacts();
```

First request authorization, and, if granted, request the contacts.

```javascript
pg.requestAccess().then((granted) => {
  if(granted !== true)
  {
    return; 
  }

  pg.getContactsCount().then( (count) => {
    pg.getContactsWithRange(0, count, [PagedContacts.displayName, PagedContacts.thumbnailImageData, PagedContacts.phoneNumbers, PagedContacts.emailAddresses]).then((contacts) => {
      //Use contacts here
    });
  });
});
```

This is a very intensive way of obtaining specific keys of all contacts. Instead, use the paging mechanism to obtain contacts within a range, and only request keys you need.

###Example of a Contact Result

```json
{
  "familyName": "Zakroff",
  "nonGregorianBirthday": "1961-12-25T22:00:00.000Z",
  "birthday": "1961-12-26T00:00:00.000Z",
  "contactRelations": [
    {
      "label": "sister",
      "value": "Kate Bell"
    }
  ],
  "nickname": "Hanky Panky",
  "displayName": "Prof. Hank M. Zakroff Esq.",
  "organizationName": "Financial Services Inc.",
  "departmentName": "Legal",
  "namePrefix": "Prof.",
  "nameSuffix": "Esq.",
  "socialProfiles": [
    {
      "label": "twitter",
      "value": {
        "urlString": "http:\/\/twitter.com\/HankyPanky",
        "username": "HankyPanky",
        "service": "Twitter"
      }
    },
    {
      "label": "facebook",
      "value": {
        "urlString": "http:\/\/www.facebook.com\/HankZakoff",
        "username": "HankZakoff",
        "service": "Facebook"
      }
    }
  ],
  "dates": [
    {
      "label": "anniversary",
      "value": "0001-12-01T00:00:00.000Z"
    },
    {
      "label": "other",
      "value": "2014-09-25T00:00:00.000Z"
    }
  ],
  "phoneNumbers": [
    {
      "label": "work",
      "value": "(555) 766-4823"
    },
    {
      "label": "other",
      "value": "(707) 555-1854"
    }
  ],
  "identifier": "60CB0169-0747-4494-9F10-22F387226676",
  "urlAddresses": [
    {
      "label": "homepage",
      "value": "https:\/\/google.com"
    }
  ],
  "postalAddresses": [
    {
      "label": "work",
      "value": {
        "ISOCountryCode": "us",
        "state": "CA",
        "street": "1741 Kearny Street",
        "city": "San Rafael",
        "country": "",
        "postalCode": "94901"
      }
    },
    {
      "label": "home",
      "value": {
        "ISOCountryCode": "il",
        "state": "",
        "street": "151 Jerusalem Avenue",
        "city": "Tel Aviv - Jaffa",
        "country": "Israel",
        "postalCode": "68152"
      }
    }
  ],
  "middleName": "M.",
  "jobTitle": "Partner",
  "note": "Best lawyer ever!",
  "emailAddresses": [
    {
      "label": "work",
      "value": "hank-zakroff@mac.com"
    }
  ],
  "givenName": "Hank",
  "instantMessageAddresses": [
    {
      "label": "Facebook",
      "value": {
        "service": "Facebook",
        "username": "HankZakoff"
      }
    },
    {
      "label": "Skype",
      "value": {
        "service": "Skype",
        "username": "HZakoff"
      }
    }
  ]
}
```