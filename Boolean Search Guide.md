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

# Best Practices

### DO ✅

**Test queries incrementally**

* Start broad, add filters gradually

* Check result counts at each step

**Use wildcards for word variations**

threat\* instead of threat OR threats OR threatening

**Use proximity search for related concepts**

"violent rhetoric"~10 catches various phrasings

**Exclude noise with NOT**

Add NOT (satire OR parody OR joke) to serious threat searches

**Use parentheses for complex logic**

Makes queries readable and ensures correct logic

**Combine operators strategically**

Mix exact phrases, wildcards, and Boolean logic

### DON'T ❌

**Don't use excessive wildcards**

❌ \*kill\* (searches everything)

✅ kill\* (searches from start)

**Don't use high fuzziness without reason**

❌ term~5 (very slow, too permissive)

✅ term~1 or term~2 (fast, reasonable)

**Don't forget parentheses in complex queries**

❌ kill OR hang AND them (ambiguous)

✅ (kill OR hang) AND them (clear)

**Don't make queries too broad**

❌ violent (returns everything)

✅ violent AND trump NOT satire (focused)

**Don't ignore false positives**

Always add exclusions for entertainment/news/satire

# Last updated:
November 25, 2025
