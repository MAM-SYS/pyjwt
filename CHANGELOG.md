Change Log
=========================================================================

All notable changes to this project will be documented in this file.
This project adheres to [Semantic Versioning](https://semver.org/).

[Unreleased][unreleased]
-------------------------------------------------------------------------
### Changed

### Fixed

### Added

[v2.0.0][2.0.0]
-------------------------------------------------------------------------
### Changed

#### Drop support for Python 2 and Python 3.0-3.5

Python 3.5 is EOL so we decide to drop its support. Version `1.7.1` is the last one supporting Python 3.0-3.5.

#### Require cryptography >= 3

#### Drop support for PyCrypto and ECDSA

We've kept this around for a long time, mostly for environments that didn't allow installing cryptography.

#### Drop CLI

Dropped the included cli entry point.

#### Improve typings

We no longer need to use mypy Python 2 compatibility mode (comments)

#### `jwt.encode(...)` return type

Tokens are returned as string instead of a byte string

#### Dropped deprecated errors

Removed `ExpiredSignature`, `InvalidAudience`, and `InvalidIssuer`. Use `ExpiredSignatureError`, `InvalidAudienceError`, and `InvalidIssuerError` instead.

#### Dropped deprecated `verify_expiration` param in `jwt.decode(...)`

Use `jwt.decode(encoded, key, algorithms=["HS256"], options={"verify_exp": False})` instead.

#### Dropped deprecated `verify` param in `jwt.decode(...)`

Use `jwt.decode(encoded, key, options={"verify_signature": False})` instead.

#### Require explicit `algorithms` in `jwt.decode(...)` by default

Example: `jwt.decode(encoded, key, algorithms=["HS256"])`.

#### Dropped deprecated `require_*` options in `jwt.decode(...)`

For example, instead of `jwt.decode(encoded, key, algorithms=["HS256"], options={"require_exp": True})`, use `jwt.decode(encoded, key, algorithms=["HS256"], options={"require": ["exp"]})`.

### Added

#### Introduce better experience for JWKs

Introduce `PyJWK`, `PyJWKSet`, and `PyJWKClient`.

```python
import jwt
from jwt import PyJWKClient

token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik5FRTFRVVJCT1RNNE16STVSa0ZETlRZeE9UVTFNRGcyT0Rnd1EwVXpNVGsxUWpZeVJrUkZRdyJ9.eyJpc3MiOiJodHRwczovL2Rldi04N2V2eDlydS5hdXRoMC5jb20vIiwic3ViIjoiYVc0Q2NhNzl4UmVMV1V6MGFFMkg2a0QwTzNjWEJWdENAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vZXhwZW5zZXMtYXBpIiwiaWF0IjoxNTcyMDA2OTU0LCJleHAiOjE1NzIwMDY5NjQsImF6cCI6ImFXNENjYTc5eFJlTFdVejBhRTJINmtEME8zY1hCVnRDIiwiZ3R5IjoiY2xpZW50LWNyZWRlbnRpYWxzIn0.PUxE7xn52aTCohGiWoSdMBZGiYAHwE5FYie0Y1qUT68IHSTXwXVd6hn02HTah6epvHHVKA2FqcFZ4GGv5VTHEvYpeggiiZMgbxFrmTEY0csL6VNkX1eaJGcuehwQCRBKRLL3zKmA5IKGy5GeUnIbpPHLHDxr-GXvgFzsdsyWlVQvPX2xjeaQ217r2PtxDeqjlf66UYl6oY6AqNS8DH3iryCvIfCcybRZkc_hdy-6ZMoKT6Piijvk_aXdm7-QQqKJFHLuEqrVSOuBqqiNfVrG27QzAPuPOxvfXTVLXL2jek5meH6n-VWgrBdoMFH93QEszEDowDAEhQPHVs0xj7SIzA"
kid = "NEE1QURBOTM4MzI5RkFDNTYxOTU1MDg2ODgwQ0UzMTk1QjYyRkRFQw"
url = "https://dev-87evx9ru.auth0.com/.well-known/jwks.json"

jwks_client = PyJWKClient(url)
signing_key = jwks_client.get_signing_key_from_jwt(token)

data = jwt.decode(
    token,
    signing_key.key,
    algorithms=["RS256"],
    audience="https://expenses-api",
    options={"verify_exp": False},
)
print(data)
```

#### Support for JWKs containing ECDSA keys

#### Add support for Ed25519 / EdDSA

### Pull Requests

- Add PyPy3 to the test matrix (#550) by @jdufresne
- Require tweak (#280) by @psafont
- Decode return type is dict[str, Any] (#393) by @jacopofar
- Fix linter error in test_cli (#414) by @jaraco
- Run mypy with tox (#421) by @jpadilla
- Document (and prefer) pyjwt[crypto] req format (#426) by @gthb
- Correct type for json_encoder argument (#438) by @jdufresne
- Prefer https:// links where available (#439) by @jdufresne
- Pass python_requires argument to setuptools (#440) by @jdufresne
- Rename [wheel] section to [bdist_wheel] as the former is legacy (#441) by @jdufresne
- Remove setup.py test command in favor of pytest and tox (#442) by @jdufresne
- Fix mypy errors (#449) by @jpadilla
- DX Tweaks (#450) by @jpadilla
- Add support of python 3.8 (#452) by @Djailla
- Fix 406 (#454) by @justinbaur
- Add support for Ed25519 / EdDSA, with unit tests (#455) by @Someguy123
- Remove Python 2.7 compatibility (#457) by @Djailla
- Fix simple typo: encododed -> encoded (#462) by @timgates42
- Enhance tracebacks. (#477) by @JulienPalard
- Simplify `python_requires` (#478) by @michael-k
- Document top-level .encode and .decode to close #459 (#482) by @dimaqq
- Improve documentation for audience usage (#484) by @CorreyL
- Correct README on how to run tests locally (#489) by @jdufresne
- Fix `tox -e lint` warnings and errors (#490) by @jdufresne
- Run pyupgrade across project to use modern Python 3 conventions (#491) by @jdufresne
- Add Python-3-only trove classifier and remove "universal" from wheel (#492) by @jdufresne
- Emit warnings about user code, not pyjwt code (#494) by @mgedmin
- Move setup information to declarative setup.cfg (#495) by @jdufresne
- CLI options for verifying audience and issuer (#496) by @GeoffRichards
- Specify the target Python version for mypy (#497) by @jdufresne
- Remove unnecessary compatibility shims for Python 2 (#498) by @jdufresne
- Setup GH Actions (#499) by @jpadilla
- Implementation of ECAlgorithm.from_jwk (#500) by @jpadilla
- Remove cli entry point (#501) by @jpadilla
- Expose InvalidKeyError on jwt module (#503) by @russellcardullo
- Avoid loading token twice in pyjwt.decode (#506) by @CaselIT
- Default links to stable version of documentation (#508) by @salcedo
- Update README.md badges (#510) by @jpadilla
- Introduce better experience for JWKs (#511) by @jpadilla
- Fix tox conditional extras (#512) by @jpadilla
- Return tokens as string not bytes (#513) by @jpadilla
- Drop support for legacy contrib algorithms (#514) by @jpadilla
- Drop deprecation warnings (#515) by @jpadilla
- Update Auth0 sponsorship link (#519) by @Sambego
- Update return type for jwt.encode (#521) by @moomoolive
- Run tests against Python 3.9 and add trove classifier (#522) by @michael-k
- Removed redundant `default_backend()` (#523) by @rohitkg98
- Documents how to use private keys with passphrases (#525) by @rayluo
- Update version to 2.0.0a1 (#528) by @jpadilla
- Fix usage example (#530) by @nijel
- add EdDSA to docs (#531) by @CircleOnCircles
- Remove support for EOL Python 3.5 (#532) by @jdufresne
- Upgrade to isort 5 and adjust configurations (#533) by @jdufresne
- Remove unused argument "verify" from PyJWS.decode() (#534) by @jdufresne
- Update typing syntax and usage for Python 3.6+ (#535) by @jdufresne
- Run pyupgrade to simplify code and use Python 3.6 syntax (#536) by @jdufresne
- Drop unknown pytest config option: strict (#537) by @jdufresne
- Upgrade black version and usage (#538) by @jdufresne
- Remove "Command line" sections from docs (#539) by @jdufresne
- Use existing key_path() utility function throughout tests (#540) by @jdufresne
- Replace force_bytes()/force_unicode() in tests with literals (#541) by @jdufresne
- Remove unnecessary Unicode decoding before json.loads() (#542) by @jdufresne
- Remove unnecessary force_bytes() calls priot to base64url_decode() (#543) by @jdufresne
- Remove deprecated arguments from docs (#544) by @jdufresne
- Update code blocks in docs (#545) by @jdufresne
- Refactor jwt/jwks_client.py without requests dependency (#546) by @jdufresne
- Tighten bytes/str boundaries and remove unnecessary coercing (#547) by @jdufresne
- Replace codecs.open() with builtin open() (#548) by @jdufresne
- Replace int_from_bytes() with builtin int.from_bytes() (#549) by @jdufresne
- Enforce .encode() return type using mypy (#551) by @jdufresne
- Prefer direct indexing over options.get() (#552) by @jdufresne
- Cleanup "noqa" comments (#553) by @jdufresne
- Replace merge_dict() with builtin dict unpacking generalizations (#555) by @jdufresne
- Do not mutate the input payload in PyJWT.encode() (#557) by @jdufresne
- Use direct indexing in PyJWKClient.get_signing_key_from_jwt() (#558) by @jdufresne
- Split PyJWT/PyJWS classes to tighten type interfaces   (#559) by @jdufresne
- Simplify mocked_response test utility function (#560) by @jdufresne
- Autoupdate pre-commit hooks and apply them (#561) by @jdufresne
- Remove unused argument "payload" from PyJWS._verify_signature() (#562) by @jdufresne
- Add utility functions to assist test skipping (#563) by @jdufresne
- Type hint jwt.utils module (#564) by @jdufresne
- Prefer ModuleNotFoundError over ImportError (#565) by @jdufresne
- Fix tox "manifest" environment to pass (#566) by @jdufresne
- Fix tox "docs" environment to pass (#567) by @jdufresne
- Simplify black configuration to be closer to upstream defaults (#568) by @jdufresne
- Use generator expressions (#569) by @jdufresne
- Simplify from_base64url_uint() (#570) by @jdufresne
- Drop lint environment from GitHub actions in favor of pre-commit.ci (#571) by @jdufresne
- [pre-commit.ci] pre-commit autoupdate (#572)
- Simplify tox configuration (#573) by @jdufresne
- Combine identical test functions using pytest.mark.parametrize() (#574) by @jdufresne
- Complete type hinting of jwks_client.py (#578) by @jdufresne

[v1.7.1][1.7.1]
-------------------------------------------------------------------------

### Fixed

- Update test dependencies with pinned ranges
- Fix pytest deprecation warnings

[v1.7.0][1.7.0]
-------------------------------------------------------------------------

### Changed

- Remove CRLF line endings [#353][353]

### Fixed

- Update usage.rst [#360][360]

### Added

- Support for Python 3.7 [#375][375] [#379][379] [#384][384]

[v1.6.4][1.6.4]
-------------------------------------------------------------------------
### Fixed

- Reverse an unintentional breaking API change to .decode() [#352][352]

[v1.6.3][1.6.3]
-------------------------------------------------------------------------
### Changed

- All exceptions inherit from PyJWTError [#340][340]

### Added

- Add type hints [#344][344]
- Add help module [7ca41e][7ca41e]

### Docs

- Added section to usage docs for jwt.get_unverified_header() [#350][350]
- Update legacy instructions for using pycrypto [#337][337]

[v1.6.1][1.6.1]
-------------------------------------------------------------------------
### Fixed

- Audience parameter throws `InvalidAudienceError` when application does not specify an audience, but the token does. [#336][336]

[v1.6.0][1.6.0]
-------------------------------------------------------------------------
### Changed

- Dropped support for python 2.6 and 3.3 [#301][301]
- An invalid signature now raises an `InvalidSignatureError` instead of `DecodeError` [#316][316]

### Fixed

- Fix over-eager fallback to stdin [#304][304]

### Added

- Audience parameter now supports iterables [#306][306]

[v1.5.3][1.5.3]
-------------------------------------------------------------------------
### Changed

- Increase required version of the cryptography package to >=1.4.0.

### Fixed

- Remove uses of deprecated functions from the cryptography package.
- Warn about missing `algorithms` param to `decode()` only when `verify` param is `True` [#281][281]

[v1.5.2][1.5.2]
-------------------------------------------------------------------------
### Fixed

- Ensure correct arguments order in decode super call [7c1e61d][7c1e61d]

[v1.5.1][1.5.1]
-------------------------------------------------------------------------
### Changed

- Change optparse for argparse. [#238][238]

### Fixed

- Guard against PKCS1 PEM encoded public keys [#277][277]
- Add deprecation warning when decoding without specifying `algorithms` [#277][277]
- Improve deprecation messages [#270][270]
- PyJWT.decode: move verify param into options [#271][271]

### Added

- Support for Python 3.6 [#262][262]
- Expose jwt.InvalidAlgorithmError [#264][264]

[v1.5.0][1.5.0]
-------------------------------------------------------------------------
### Changed
- Add support for ECDSA public keys in RFC 4253 (OpenSSH) format [#244][244]
- Renamed commandline script `jwt` to `jwt-cli` to avoid issues with the script clobbering the `jwt` module in some circumstances. [#187][187]
- Better error messages when using an algorithm that requires the cryptography package, but it isn't available [#230][230]
- Tokens with future 'iat' values are no longer rejected [#190][190]
- Non-numeric 'iat' values now raise InvalidIssuedAtError instead of DecodeError
- Remove rejection of future 'iat' claims [#252][252]

### Fixed
- Add back 'ES512' for backward compatibility (for now) [#225][225]
- Fix incorrectly named ECDSA algorithm [#219][219]
- Fix rpm build [#196][196]

### Added
- Add JWK support for HMAC and RSA keys [#202][202]

[v1.4.2][1.4.2]
-------------------------------------------------------------------------
### Fixed
- A PEM-formatted key encoded as bytes could cause a `TypeError` to be raised [#213][213]

[v1.4.1][1.4.1]
-------------------------------------------------------------------------
### Fixed
- Newer versions of Pytest could not detect warnings properly [#182][182]
- Non-string 'kid' value now raises `InvalidTokenError` [#174][174]
- `jwt.decode(None)` now gracefully fails with `InvalidTokenError` [#183][183]

[v1.4][1.4.0]
-------------------------------------------------------------------------
### Fixed
- Exclude Python cache files from PyPI releases.

### Added
- Added new options to require certain claims
  (require_nbf, require_iat, require_exp) and raise `MissingRequiredClaimError`
  if they are not present.
- If `audience=` or `issuer=` is specified but the claim is not present,
  `MissingRequiredClaimError` is now raised instead of `InvalidAudienceError`
  and `InvalidIssuerError`

[v1.3][1.3.0]
-------------------------------------------------------------------------
### Fixed
- ECDSA (ES256, ES384, ES512) signatures are now being properly serialized [#158][158]
- RSA-PSS (PS256, PS384, PS512) signatures now use the proper salt length for PSS padding. [#163][163]

### Added
- Added a new `jwt.get_unverified_header()` to parse and return the header portion of a token prior to signature verification.

### Removed
- Python 3.2 is no longer a supported platform. This version of Python is
rarely used. Users affected by this should upgrade to 3.3+.

[v1.2.0][1.2.0]
-------------------------------------------------------------------------
### Fixed
- Added back `verify_expiration=` argument to `jwt.decode()` that was erroneously removed in [v1.1.0][1.1.0].


### Changed
- Refactored JWS-specific logic out of PyJWT and into PyJWS superclass. [#141][141]

### Deprecated
- `verify_expiration=` argument to `jwt.decode()` is now deprecated and will be removed in a future version. Use the `option=` argument instead.

[v1.1.0][1.1.0]
-------------------------------------------------------------------------
### Added
- Added support for PS256, PS384, and PS512 algorithms. [#132][132]
- Added flexible and complete verification options during decode. [#131][131]
- Added this CHANGELOG.md file.


### Deprecated
- Deprecated usage of the .decode(..., verify=False) parameter.


### Fixed
- Fixed command line encoding. [#128][128]

[v1.0.1][1.0.1]
-------------------------------------------------------------------------
### Fixed
- Include jwt/contrib' and jwt/contrib/algorithms` in setup.py so that they will
  actually be included when installing. [882524d][882524d]
- Fix bin/jwt after removing jwt.header(). [bd57b02][bd57b02]

[v1.0.0][1.0.0]
-------------------------------------------------------------------------
### Changed
- Moved `jwt.api.header` out of the public API. [#85][85]
- Added README details how to extract public / private keys from an x509 certificate. [#100][100]
- Refactor api.py functions into an object (`PyJWT`). [#101][101]
- Added support for PyCrypto and ecdsa when cryptography isn't available. [#101][103]

### Fixed
- Fixed a security vulnerability where `alg=None` header could bypass signature verification. [#109][109]
- Fixed a security vulnerability by adding support for a whitelist of allowed `alg` values `jwt.decode(algorithms=[])`. [#110][110]


[unreleased]: https://github.com/jpadilla/pyjwt/compare/1.4.2...HEAD
[1.0.0]: https://github.com/jpadilla/pyjwt/compare/0.4.3...1.0.0
[1.0.1]: https://github.com/jpadilla/pyjwt/compare/1.0.0...1.0.1
[1.0.1]: https://github.com/jpadilla/pyjwt/compare/1.0.0...1.0.1
[1.0.1]: https://github.com/jpadilla/pyjwt/compare/1.0.0...1.0.1
[1.1.0]: https://github.com/jpadilla/pyjwt/compare/1.0.1...1.1.0
[1.2.0]: https://github.com/jpadilla/pyjwt/compare/1.1.0...1.2.0
[1.3.0]: https://github.com/jpadilla/pyjwt/compare/1.2.0...1.3.0
[1.4.0]: https://github.com/jpadilla/pyjwt/compare/1.3.0...1.4.0
[1.4.1]: https://github.com/jpadilla/pyjwt/compare/1.4.0...1.4.1
[1.4.2]: https://github.com/jpadilla/pyjwt/compare/1.4.1...1.4.2
[1.5.0]: https://github.com/jpadilla/pyjwt/compare/1.4.2...1.5.0
[1.5.1]: https://github.com/jpadilla/pyjwt/compare/1.5.0...1.5.1
[1.5.2]: https://github.com/jpadilla/pyjwt/compare/1.5.1...1.5.2
[1.5.3]: https://github.com/jpadilla/pyjwt/compare/1.5.2...1.5.3
[1.6.0]: https://github.com/jpadilla/pyjwt/compare/1.5.3...1.6.0
[1.6.1]: https://github.com/jpadilla/pyjwt/compare/1.6.0...1.6.1
[1.6.3]: https://github.com/jpadilla/pyjwt/compare/1.6.1...1.6.3
[1.6.4]: https://github.com/jpadilla/pyjwt/compare/1.6.3...1.6.4
[1.7.0]: https://github.com/jpadilla/pyjwt/compare/1.6.4...1.7.0
[1.7.1]: https://github.com/jpadilla/pyjwt/compare/1.7.0...1.7.1
[2.0.0]: https://github.com/jpadilla/pyjwt/compare/1.7.1...2.0.0

[109]: https://github.com/jpadilla/pyjwt/pull/109
[110]: https://github.com/jpadilla/pyjwt/pull/110
[100]: https://github.com/jpadilla/pyjwt/pull/100
[101]: https://github.com/jpadilla/pyjwt/pull/101
[103]: https://github.com/jpadilla/pyjwt/pull/103
[85]: https://github.com/jpadilla/pyjwt/pull/85
[882524d]: https://github.com/jpadilla/pyjwt/commit/882524d
[bd57b02]: https://github.com/jpadilla/pyjwt/commit/bd57b02
[131]: https://github.com/jpadilla/pyjwt/pull/131
[132]: https://github.com/jpadilla/pyjwt/pull/132
[128]: https://github.com/jpadilla/pyjwt/pull/128
[141]: https://github.com/jpadilla/pyjwt/pull/141
[158]: https://github.com/jpadilla/pyjwt/pull/158
[163]: https://github.com/jpadilla/pyjwt/pull/163
[174]: https://github.com/jpadilla/pyjwt/pull/174
[182]: https://github.com/jpadilla/pyjwt/pull/182
[183]: https://github.com/jpadilla/pyjwt/pull/183
[190]: https://github.com/jpadilla/pyjwt/pull/190
[213]: https://github.com/jpadilla/pyjwt/pull/214
[244]: https://github.com/jpadilla/pyjwt/pull/244
[202]: https://github.com/jpadilla/pyjwt/pull/202
[252]: https://github.com/jpadilla/pyjwt/pull/252
[225]: https://github.com/jpadilla/pyjwt/pull/225
[219]: https://github.com/jpadilla/pyjwt/pull/219
[196]: https://github.com/jpadilla/pyjwt/pull/196
[187]: https://github.com/jpadilla/pyjwt/pull/187
[230]: https://github.com/jpadilla/pyjwt/pull/230
[238]: https://github.com/jpadilla/pyjwt/pull/238
[262]: https://github.com/jpadilla/pyjwt/pull/262
[264]: https://github.com/jpadilla/pyjwt/pull/264
[270]: https://github.com/jpadilla/pyjwt/pull/270
[271]: https://github.com/jpadilla/pyjwt/pull/271
[277]: https://github.com/jpadilla/pyjwt/pull/277
[281]: https://github.com/jpadilla/pyjwt/pull/281
[301]: https://github.com/jpadilla/pyjwt/pull/301
[304]: https://github.com/jpadilla/pyjwt/pull/304
[306]: https://github.com/jpadilla/pyjwt/pull/306
[315]: https://github.com/jpadilla/pyjwt/pull/315
[316]: https://github.com/jpadilla/pyjwt/pull/316
[336]: https://github.com/jpadilla/pyjwt/pull/336
[337]: https://github.com/jpadilla/pyjwt/pull/337
[340]: https://github.com/jpadilla/pyjwt/pull/340
[344]: https://github.com/jpadilla/pyjwt/pull/344
[350]: https://github.com/jpadilla/pyjwt/pull/350
[352]: https://github.com/jpadilla/pyjwt/pull/352
[353]: https://github.com/jpadilla/pyjwt/pull/353
[360]: https://github.com/jpadilla/pyjwt/pull/360
[375]: https://github.com/jpadilla/pyjwt/pull/375
[379]: https://github.com/jpadilla/pyjwt/pull/379
[384]: https://github.com/jpadilla/pyjwt/pull/384
[7c1e61d]: https://github.com/jpadilla/pyjwt/commit/7c1e61dde27bafe16e7d1bb6e35199e778962742
[7ca41e]: https://github.com/jpadilla/pyjwt/commit/7ca41e53b3d7d9f5cd31bdd8a2b832d192006239
