# Contributing

## Terminology

* Rule - documented, identifiable guideline or error/warning/issue guidance.
* Rule identifier - `TMPRL` prefix + number. Often used interchangeably with "Rule number" which is ok.
* Rule number - numeric part of the identifier. Often used interchangeably with "Rule identifier" which is ok.
* Rule document - `rules/TMPRL<number>.md` markdown file describing the rule.
* Rule category - grouping of common set of rules with a known identifier range. There are only 3 today: SDK, server,
  and cloud.
* Rule category base number - starting rule number of the category range.

## Guidelines

Overarching guidelines:

* None of these guidelines are strict, and any can be overridden for specific use cases after discussion.
* Anything in this repo that must be parsable will be enforced via CI, but contributors should try to be reasonably
  consistent.
* Manually wrap markdown at 120 characters per line where possible.

### Rule criteria

Notes about what is best as a rule:

* Only create a rule if it deserves standalone documentation.
  * Not every error is a rule. Only errors that are commonly encountered and deserve a full document explaining them
    deserve a rule.
  * It is important for the rule list does not grow way too large due to being overused for every small thing.
  * A good rule of thumb is to not make a rule if it isn't an error or situation you would have a full documentation
    page for on docs.temporal.io.
* Prefer reusing fewer common rules over creating many specific rules.
  * For example, a single "payload conversion failure" rule for all things that could happen in either direction of
    payload conversion is much better than separate rules for to payload, from payload, metadata validation, etc.
* Rules are created and referenced by Temporal, they are not meant to be put in user code or reused by users.
* SDK-specific or environment-specific rules are OK.
  * Don't overthink the ordering of the rules within their category though. For example, not all rules of an SDK need to
    be near each other.
  * Don't make new rules to represent language-specific variants of the same rule. If necessary, the rule document can
    call out language specifics.
* Create rules for Temporal best practices, even if you can't programmatically reference them yet in something like an
  analyzer.
  * We can still link to and reference these best practices by their rule when supporting users.

### Creating a rule

* Pick an available number in the range in the category for the rule.
  * The next available number in sequence should be used, but this is not required if there is a good reason not to.
    Don't overthink number ordering, rules within a category have no adjacent number grouping expectations.
* Create a file at `rules/TMPRL<number>.md` for the rule document.
  * The `#` heading at the top of the file should be `# TMPRL<number> - <description>`.
  * The file may have the following sections but does not have to:
    * `## Cause` - simple explanation of the cause of the issue. Note, not all rules have a cause (e.g. just general
      guideline/best-practice rules).
    * `## Description` - in-depth description of the rule.
    * `## Remediation` - approaches for addressing the issue. Note, not all rules have a remediation (e.g. just general
      guideline/best-practice rules).
  * Can go into as much detail as desired, but link to canonical docs.temporal.io content where possible instead of
    re-documenting.
    * Do not use the rule document as proper documentation. Explaining an issue and its remediations is fine, but don't
      overly explain a feature that should be explained in documentation. For example, in an
      "invalid workflow definition" rule, it is not the rule document's responsibility to explain what is a valid
      workflow definition in every language.
  * Linking from one rule to another is encouraged as needed.
* Create an entry for the rule in the category's table in the `README.md` file.
  * This must link the rule and the description must be the exact text of the title sans rule number prefix.

### Referencing a rule in code

* When applying to an error, prefix the error message with `[TMPRL<number>] `.
  * Do not overthink this. Errors do not need to have their own "rule" field for programmatic abstraction, this is just
    a prefix for humans on the human message.
* When applying to an error, do not wrap existing errors just to prefix.
  * If a user error/exception has traditionally been bubbled, do not wrap it just to add a message with this number.
