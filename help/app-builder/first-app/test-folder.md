---
title: Le dossier test
description: Découvrez les types de fichiers contenus dans le dossier test pour cet exemple d’application.
landing-page-description: Découvrez Adobe Developer App Builder utilisé avec Adobe Commerce et les types de fichiers situés dans le dossier test.
kt: 12424
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 5d3ced3f-174d-4481-8511-82616bb77c43
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# En savoir plus sur le dossier test {#test-folder}

Le dossier `test` de cet exemple d’application contient un seul fichier JavaScript, utilisé lors de l’exécution de tests unitaires sur l’application.

Il s’agit d’un exemple simple qui peut être développé afin de créer des tests complets pour vos applications spécifiques.

## Pour qui est cette vidéo ?

* Les développeurs découvrent Adobe Commerce avec une expérience limitée de l’utilisation d’Adobe App Builder qui souhaitent en savoir plus sur le dossier `test`.

## Contenu vidéo

* Pourquoi utiliser le dossier `test` ?
* Une brève explication du fichier de test unitaire et de ses composants
* Présentation des tests de bout en bout

>[!VIDEO](https://video.tv.adobe.com/v/3421061?quality=12&learn=on&captions=fre_fr)

## Exemples de code

test/utils.test.js

```javascript
/* 
* <license header>
*/

const utils = require('./../actions/utils.js')

test('interface', () => {
  expect(typeof utils.errorResponse).toBe('function')
  expect(typeof utils.stringParameters).toBe('function')
  expect(typeof utils.checkMissingRequestInputs).toBe('function')
  expect(typeof utils.getBearerToken).toBe('function')
})

describe('errorResponse', () => {
  test('(400, errorMessage)', () => {
    const res = utils.errorResponse(400, 'errorMessage')
    expect(res).toEqual({
      error: {
        statusCode: 400,
        body: { error: 'errorMessage' }
      }
    })
  })

  test('(400, errorMessage, logger)', () => {
    const logger = {
      info: jest.fn()
    }
    const res = utils.errorResponse(400, 'errorMessage', logger)
    expect(logger.info).toHaveBeenCalledWith('400: errorMessage')
    expect(res).toEqual({
      error: {
        statusCode: 400,
        body: { error: 'errorMessage' }
      }
    })
  })
})

describe('stringParameters', () => {
  test('no auth header', () => {
    const params = {
      a: 1, b: 2, __ow_headers: { 'x-api-key': 'fake-api-key' }
    }
    expect(utils.stringParameters(params)).toEqual(JSON.stringify(params))
  })
  test('with auth header', () => {
    const params = {
      a: 1, b: 2, __ow_headers: { 'x-api-key': 'fake-api-key', authorization: 'secret' }
    }
    expect(utils.stringParameters(params)).toEqual(expect.stringContaining('"authorization":"<hidden>"'))
    expect(utils.stringParameters(params)).not.toEqual(expect.stringContaining('secret'))
  })
})

describe('checkMissingRequestInputs', () => {
  test('({ a: 1, b: 2 }, [a])', () => {
    expect(utils.checkMissingRequestInputs({ a: 1, b: 2 }, ['a'])).toEqual(null)
  })
  test('({ a: 1 }, [a, b])', () => {
    expect(utils.checkMissingRequestInputs({ a: 1 }, ['a', 'b'])).toEqual('missing parameter(s) \'b\'')
  })
  test('({ a: { b: { c: 1 } }, f: { g: 2 } }, [a.b.c, f.g.h.i])', () => {
    expect(utils.checkMissingRequestInputs({ a: { b: { c: 1 } }, f: { g: 2 } }, ['a.b.c', 'f.g.h.i'])).toEqual('missing parameter(s) \'f.g.h.i\'')
  })
  test('({ a: { b: { c: 1 } }, f: { g: 2 } }, [a.b.c, f.g.h])', () => {
    expect(utils.checkMissingRequestInputs({ a: { b: { c: 1 } }, f: { g: 2 } }, ['a.b.c', 'f'])).toEqual(null)
  })
  test('({ a: 1, __ow_headers: { h: 1, i: 2 } }, undefined, [h])', () => {
    expect(utils.checkMissingRequestInputs({ a: 1, __ow_headers: { h: 1, i: 2 } }, undefined, ['h'])).toEqual(null)
  })
  test('({ a: 1, __ow_headers: { f: 2 } }, [a], [h, i])', () => {
    expect(utils.checkMissingRequestInputs({ a: 1, __ow_headers: { f: 2 } }, ['a'], ['h', 'i'])).toEqual('missing header(s) \'h,i\'')
  })
  test('({ c: 1, __ow_headers: { f: 2 } }, [a, b], [h, i])', () => {
    expect(utils.checkMissingRequestInputs({ c: 1 }, ['a', 'b'], ['h', 'i'])).toEqual('missing header(s) \'h,i\' and missing parameter(s) \'a,b\'')
  })
  test('({ a: 0 }, [a])', () => {
    expect(utils.checkMissingRequestInputs({ a: 0 }, ['a'])).toEqual(null)
  })
  test('({ a: null }, [a])', () => {
    expect(utils.checkMissingRequestInputs({ a: null }, ['a'])).toEqual(null)
  })
  test('({ a: \'\' }, [a])', () => {
    expect(utils.checkMissingRequestInputs({ a: '' }, ['a'])).toEqual('missing parameter(s) \'a\'')
  })
  test('({ a: undefined }, [a])', () => {
    expect(utils.checkMissingRequestInputs({ a: undefined }, ['a'])).toEqual('missing parameter(s) \'a\'')
  })
})

describe('getBearerToken', () => {
  test('({})', () => {
    expect(utils.getBearerToken({})).toEqual(undefined)
  })
  test('({ authorization: Bearer fake, __ow_headers: {} })', () => {
    expect(utils.getBearerToken({ authorization: 'Bearer fake', __ow_headers: {} })).toEqual(undefined)
  })
  test('({ authorization: Bearer fake, __ow_headers: { authorization: fake } })', () => {
    expect(utils.getBearerToken({ authorization: 'Bearer fake', __ow_headers: { authorization: 'fake' } })).toEqual(undefined)
  })
  test('({ __ow_headers: { authorization: Bearerfake} })', () => {
    expect(utils.getBearerToken({ __ow_headers: { authorization: 'Bearerfake' } })).toEqual(undefined)
  })
  test('({ __ow_headers: { authorization: Bearer fake} })', () => {
    expect(utils.getBearerToken({ __ow_headers: { authorization: 'Bearer fake' } })).toEqual('fake')
  })
  test('({ __ow_headers: { authorization: Bearer fake Bearer fake} })', () => {
    expect(utils.getBearerToken({ __ow_headers: { authorization: 'Bearer fake Bearer fake' } })).toEqual('fake Bearer fake')
  })
})
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
