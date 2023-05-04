# KDID Method Specification

## About

KDID(KingDom Decentralized Identity) is based on the blockchain and provides distributed entity identity and management functions. KDID is an infrastructure that committed to solve cross-agency and cross-industry identity authentication and data exchange issues to realizing value exchange especially in the financial field.

## Status Of This Document

This document is now version 1.0.0 and will be further updated following the W3C DIDs Recommendation if necessary.

## KDID Method

The DID Identifier of KingDom Distributed Identity is defined as: kdid. KDID is created by the following ABNF format defined in RFC2234.

```apache
kdid = “did:kdid:” kdid-specific-identifier
kdid-specific-identifier = 0*1(bst “:”) suffix
bst = [1*6(ALPHA / DIGIT)] 0*1(":”) [1*10(ALPHA / DIGIT)]
suffix = (10,40)( ALPHA / DIGIT / “.”)
```

```text
#example of kdid
did:kdid:beijing:Q123456789
#Scheme
“did”
#DID Method
“kdid”
#DID Method Specific Identifier
beijing:Q123456789
```

## KDID Document

A KDID Document is a set of data describing a KDID subject and the serialized result of a DID Document should be a JSON-LD structure. Elements of a KDID Document:
@context: required field, explanation of JSON-LD, following DID documentation.

- Id: required field, target KDID with prefix “did:kdid:”.
- alsoKnownAs: optional field, a list of entity identifier.
- controller: required field, indicating the ownership of the did document.
- verificationMethod: required field, a list of Verification Methods, which can be used to verify or authorize interactions with DID entity or related parties.
- Authentication: required field, the attribute value is a set of validation methods, which can be defined inline in the validation relationship or directly referenced by the Id to the verificationMethod
- assertionMethod: optional field, the attribute value of is a set of validation methods that can be defined inline in the validation relationship or directly referenced through an Id.
- service: optional, used to indicate how to communicate with the DID principal or associated entity.

An example DID Document of did:kdid:beijing:Q123456789 is

```json
{
  "@context": [
    "https://w3id.org/did/v1"
  ],
  "id": " did:kdid:beijing:Q123456789",
  "alsoKnownAs": [
    "https://test.kdid.com/"
  ],
  "controller": [
    " did:kdid:beijing:Q123456789"
  ],
  "verificationMethod": [
    {
      "id": " did:kdid:beijing:Q123456789#keys-1",
      "type": "SM2VerificationKey2022",
      "controller": [
        " did:kdid:beijing:Q123456789"
      ],
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "SM2",
        "x": "Jrfy7ixuhqpIaLTK8ZRLc3cUFr9yXCZZtqS-gNQcBAM",
        "y": "B7sJ8qFlhw2EoMSl1okiSmTfICpyPj2RZ9H00ap-AcY"
      }
    }
  ],
  "authentication": [
    " did:kdid:beijing:Q123456789#keys-1",
    {
      "id": " did:kdid:beijing:Q123456789#keys-2",
      "type": "SM2VerificationKey2022",
      "controller": "did:kdid:beijing:Q123456789",
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "SM2",
        "x": "gQnAOAhy86yEhmzjyzl9WCLAZsl7an3soSP9DMQKzq8",
        "y": "bmKEORjcRfofE9nOGSUCFG_GbTYY3CWPAKJV2yWg0pc"
      }
    }
  ],
  "assertionMethod": [
    " did:kdid:beijing:Q123456789#keys-1",
    {
      "id":"did:kdid:beijing:Q123456789#keys-2",
      "type":"SM2VerificationKey2022",
      "controller":[
        "did:kdid:beijing:Q123456789"
      ],
      "publicKeyJwk":{
        "kty":"EC",
        "crv":"SM2",
        "x":"BrNV5ycOAPFNXmha1fzt583M3bXWd9lpQPW5AcO8wEU",
        "y":"y_6KYDflQcF0jL6L80CnSbP80KxWMc1xWv3cd5DVpVc"
      }
    }
  ],
  "service": [
    {
      "id": "did:kdid:beijing:Q123456789#linked-domain",
      "type": "LinkedDomains",
      "serviceEndPoint": [
        "https://www.kdid.com"
      ]
    }
  ]
}
```

## KDID Operations

KDID functions are implemented by smart contract and DID Documents are saved in blockchain. To create DID Document, system will first generate a standards-compliant kdid under smart contract based on user inputs, and then assemble a default Document for this kdid by generating a key pair etc. In the last, system will submit a transaction to blockchain to store the Document.

### Create

```text
#endpoint
the kdid service api url

# Input
{
    kdid
}

# Output
{create result}
```

Example:

#### input

```json
{
  "kdid": "did:kdid:beijing:Q123456789"
}
```

#### output

```json
{
  "code": 200,
  "message": null,
  "data": " did:kdid:beijing:Q123456789"
}
```

### Read

```text
# endpoint
the kdid service api url

# Input
{
    kdid
}

# Output
{kdid Document}
```

Example:

#### input

```json
{
  "kdid": "did:kdid:beijing:Q123456789"
}
```

#### output

```json
{
  "code": 200,
  "message": null,
  "data": {
    "documentMetadata": {
      "created": "2023-03-21T02:17:56Z",
      "updated": "2023-03-21T02:24:34Z",
      "deactivated": false,
      "versionId": "v1"
    },
    "document": {
      "@context": [
        "https://w3id.org/did/v1"
      ],
      "id": " did:kdid:beijing:Q123456789",
      "alsoKnownAs": [
        "https://test.kdid.com/"
      ],
      "controller": [
        " did:kdid:beijing:Q123456789"
      ],
      "verificationMethod": [
        {
          "id": " did:kdid:beijing:Q123456789#keys-1",
          "type": "SM2VerificationKey2022",
          "controller": [
            " did:kdid:beijing:Q123456789"
          ],
          "publicKeyJwk": {
            "kty": "EC",
            "crv": "SM2",
            "x": "dhCiHzNH6vjBA38dp30S3lBAjCl-Veyqm5vrLe1rd-k",
            "y": "J-OehMsX_2jvk2f8Xtly2aoVfP-zu3rgWcLbiDKyWEI"
          }
        }
      ],
      "authentication": [
        " did:kdid:beijing:Q123456789#keys-1"
      ],
      "assertionMethod": [
        " did:kdid:beijing:Q123456789#keys-1"
      ],
      "service": []
    }
  }
}
```

### Update

```text
# endpoint
the kdid service api url

# Input
{
    updateContent
}

# Output
{update result}
```

Example:

#### input

```json
{
  "document": {
    "@context": [
      "https://w3id.org/did/v1"
    ],
    "id": "did:kdid:beijing:QR357999286",
    "alsoKnownAs": [
      "https://test.kdid.com/"
    ],
    "controller": [
      "did:kdid:beijing:QR357999286"
    ],
    "verificationMethod": [
      {
        "id": "did:kdid:beijing:QR357999286#keys-1",
        "type": "SM2VerificationKey2022",
        "controller": [
          "did:kdid:beijing:QR357999286"
        ],
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "SM2",
          "x": "gQnAOAhy86yEhmzjyzl9WCLAZsl7an3soSP9DMQKzq8",
          "y": "bmKEORjcRfofE9nOGSUCFG_GbTYY3CWPAKJV2yWg0pc"
        }
      }
    ],
    "authentication": [
      "did:kdid:beijing:QR357999286#keys-1",
      {
        "id": "did:kdid:beijing:QR357999286#keys-2",
        "type": "SM2VerificationKey2022",
        "controller": [
          "did:kdid:beijing:QR357999286"
        ],
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "SM2",
          "x": "gQnAOAhy86yEhmzjyzl9WCLAZsl7an3soSP9DMQKzq8",
          "y": "bmKEORjcRfofE9nOGSUCFG_GbTYY3CWPAKJV2yWg0pc"
        }
      }
    ],
    "assertionMethod": [
      "did:kdid:beijing:QR357999286#keys-1",
      {
        "id": "did:kdid:beijing:QR357999286#keys-2",
        "type": "SM2VerificationKey2022",
        "controller": [
          "did:kdid:beijing:QR357999286"
        ],
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "SM2",
          "x": "gQnAOAhy86yEhmzjyzl9WCLAZsl7an3soSP9DMQKzq8",
          "y": "bmKEORjcRfofE9nOGSUCFG_GbTYY3CWPAKJV2yWg0pc"
        }
      }
    ],
    "service": [
      {
        "id": "did:kdid:beijing:QR357999286#linked-domain",
        "type": "LinkedDomains",
        "serviceEndPoint": [
          "https://www.kdid.com"
        ]
      }
    ]
  }
}
```

#### output

```json
{
  "code": 200,
  "message": null,
  "data": "null"
}
```

### Deactivate

```text
# endpoint
the kdid service api url

# Input
{
    kdid
}

# Output
{deactivate result}
```

Example:

#### input

```json
{
  "kdid": "did:kdid:beijing:Q323162601"
}
```

#### output

```json
{
  "code": 200,
  "message": null,
  "data": "null"
}
```

## Security considerations

1. KDID is based on the block chain, which has a natural advantage in defending against DDOS attacks due to its technical characteristics.
2. Eavesdropping attacks are not applicable since all exchanged data is public and does not include any personal information about the user.

## Privacy Considerations

1. There is no any personal information about the user in Document, which only saves user’s public key on the block chain.
2. All user-related privacy data is stored locally when entity issues verifiable credentials.
3. The user's verifiable credentials are stored under their system account and can only be read when password verification is passed.
4. Useing selective disclosure to achieve privacy data protection for credential holders.
5. Using cryptography techniques such as zero knowledge proof to avoid reverse reasoning through massive amounts of data to discover the mapping relationship between DID identifiers, and deduce the personal identity of the physical world when a third party collects sufficient personal DID data.
