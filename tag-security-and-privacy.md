# TAG Review: Security and Privacy questionnaire

## 2.1. What information might this feature expose to Web sites or other parties, and for what purposes is that exposure necessary?

This proposal only exposes the information to web servers that a particular resource is being fetched. This is necessary to load the module graph.

The current proposal does not send any headers related to the import condition/module type for content negotiation, though this has been raised in [tc39/proposal-import-conditions#61](https://github.com/tc39/proposal-import-conditions/issues/61).


## 2.2. Is this specification exposing the minimum amount of information necessary to power the feature?

Yes.

## 2.3. How does this specification deal with personal information or personally-identifiable information or information derived thereof?

HTML imports modules by performing fetches from the URL indicated in the module specifier. JavaScript code may construct a URL exposing personally identifying information which is implicitly communicated by importing a module with that URL, but this is already possible with JS modules.

No new identifying information is communicated by this proposal.

## 2.4. How does this specification deal with sensitive information

No extra sensitive information is exposed by this proposal.

## 2.5. Does this specification introduce new state for an origin that persists across browsing sessions?

No.

## 2.6. What information from the underlying platform, e.g. configuration data, is exposed by this specification to an origin?

None.

## 2.7. Does this specification allow an origin access to sensors on a user’s device

No.

## 2.8. What data does this specification expose to an origin? Please also document what data is identical to data exposed by other features, in the same or different contexts.

No data.

## 2.9. Does this specification enable new script execution/loading mechanisms?

This proposal will enable future module types on the web, first JSON module and then HTML, CSS, ...

This will be done via the `type` attribute, which is designed to let the importer specify whether the imported module has the capability to execute code: some module types are able to execute code, while others are not.

## 2.10. Does this specification allow an origin to access other devices?

No.

## 2.11. Does this specification allow an origin some measure of control over a user agent’s native UI?

No.

## 2.12. What temporary identifiers might this specification create or expose to the web?

No identifiers.

## 2.13. How does this specification distinguish between behavior in first-party and third-party contexts?

This specification allows importing more kinds of cross-origin subresources as modules, analogous to how ES modules work. The imported subresources are not distinguished and generally treated as "first-party", but each new subresource types will be importable only with the explicit use of a `type` assertion, which avoids giving the subresource unnecessary capabilities (including both executing code and accessing parsers).

## 2.14. How does this specification work in the context of a user agent’s Private Browsing or "incognito" mode?

No difference.

## 2.15. Does this specification have a "Security Considerations" and "Privacy Considerations" section?

Part of the proposal motivation is the security aspect, as explained in the [README.md#motivation](https://github.com/tc39/proposal-import-conditions/blob/master/README.md#motivation).
We consider that there are no particular privacy considerations.

## 2.16. Does this specification allow downgrading default security characteristics?

No.

## 2.17. What should this questionnaire have asked?

The questions seem adequate.
