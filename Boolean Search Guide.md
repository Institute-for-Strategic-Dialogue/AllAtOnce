# AllAtOnce Boolean Search Documentation

**Search Operators Reference**

### 1\. Basic Boolean Operators

Standard Boolean logic for combining search terms:

| **Operator** | **Example** | **Result** |
| --- | --- | --- |
| **OR** | kill OR hang | Posts with either term |
| **AND** | violent AND trump | Posts with both terms |
| **NOT** | threat NOT satire | Posts with first term but not second |
| **( )** | (kill OR hang) AND them | Group terms to control logic |
| **" "** | "political violence" | Exact word sequence |

**Complex example:**

(kill OR hang) AND (politician OR elected) NOT (satire OR parody)

This finds posts mentioning killing/hanging politicians, excluding mentions of satire/parody.

### 2\. Wildcard Search

**Multi-Character Wildcard (\*)**

Matches zero or more characters.

**Syntax:** term\*

**Examples:**

threat\* → matches: threat, threats, threatening, threatened

violen\* → matches: violent, violence, violently

**Single-Character Wildcard (?)**

Matches exactly one character.

**Syntax:** te?m

**Examples:**

wom?n → matches: woman, women

thre?t → matches: threat, threot (typo)

\------

### 3\. Proximity Search

**Find Terms Within Distance**

Match phrases where words appear within N words of each other (in any order).

**Syntax:** "term1 term2"~N

**Examples:**

"kill politician"~10 → finds "kill" and "politician" within 10 words

"trump threaten"~5 → "trump" and "threaten" within 5 words

"trump threaten"~5 OR "kill politician"~5 → "trump" and "threaten" - as well as "kill" and "politician" within 5 words

**How it works:**

"kill politician"~10 matches:

✅ "I will kill that corrupt politician"

✅ "The politician should be killed by patriots"

✅ "Kill all the lying politicians"

❌ "Politicians are corrupt. Someone should kill the mayor" (too far apart)

**Use case:** Find related concepts that appear near each other but not necessarily in exact phrase form. Especially useful for detecting threatening rhetoric where exact phrasing varies.

### 4\. Fuzzy Search

**Match Similar Terms (Typos/Variations)**

Find terms with up to N character differences (insertions, deletions, substitutions).

**Syntax:** term~N (default N=2 if omitted)

**Examples:**

politician~1 → matches: politician, politican (typo), politicians

threaten~2 → matches: threaten, threatn, threating, threatening

threaten~ → same as threaten~2. matches: threaten, threatn, threating, threatening

**Fuzziness levels:**

~1 = 1 character difference (strict)

~2 = 2 character differences (moderate, default)

~3 = too much. Will not work.

**Use case:** Catch typos, misspellings, or deliberate obfuscation (e.g., "k!ll" vs "kill").

⚠️ **Warning:** Higher fuzziness values (3 or more) will likely lead to an error.

**Last updated:** November 25, 2025
