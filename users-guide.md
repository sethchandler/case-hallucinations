# Legal Citation Hallucination Detector - User's Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Scenario 1: Reviewing an Appellate Brief](#scenario-1-reviewing-an-appellate-brief)
4. [Scenario 2: Checking a Law Review Article](#scenario-2-checking-a-law-review-article)
5. [Understanding the Results](#understanding-the-results)
6. [Best Practices](#best-practices)
7. [Troubleshooting](#troubleshooting)

## Introduction

The Legal Citation Hallucination Detector is a web-based tool designed to help legal professionals identify potentially problematic case citations in their documents. Whether you're concerned about AI-generated content, checking a junior associate's work, or simply want an extra layer of verification, this tool provides a systematic way to validate legal citations against the CourtListener database.

### What This Tool Can Do
- Detect completely fabricated case citations
- Identify real citations attributed to the wrong case names
- Flag citations that may have formatting issues
- Provide actionable editing guides for fixing problematic citations

### What This Tool Cannot Do
- Verify that a case actually supports the proposition for which it's cited
- Check statutes, regulations, or secondary sources
- Resolve "Id." or "supra" references
- Guarantee 100% accuracy (both false positives and false negatives occur)

## Getting Started

### Step 1: Obtain Your API Key
1. Visit [CourtListener Sign In](https://www.courtlistener.com/sign-in/)
2. Create a free account (takes 2 minutes)
3. Navigate to your profile settings
4. Generate an API token
5. Copy the token - you'll need it for the tool

### Step 2: Prepare Your Document
The tool accepts several file formats, but text files work most reliably:

**For Word Documents:**
1. Open your document in Microsoft Word
2. Select all text (Ctrl+A or Cmd+A)
3. Copy (Ctrl+C or Cmd+C)
4. Open a plain text editor (Notepad on Windows, TextEdit on Mac)
5. Paste the content
6. Save as a .txt file

**For PDFs:**
1. Open in Adobe Acrobat or your PDF reader
2. Select all text (Ctrl+A or Cmd+A)
3. Copy and paste into a text editor
4. Save as .txt file

**File Size Limits:**
- Maximum 64,000 characters (approximately 40-50 pages)
- Maximum 250 citations per document
- For longer documents, split into sections

## Scenario 1: Reviewing an Appellate Brief

### The Situation
You're a supervising attorney reviewing a 35-page appellate brief drafted by a junior associate who used AI assistance for initial research. You want to ensure all case citations are real and correctly attributed.

### Step-by-Step Process

#### 1. Document Preparation
Your brief is in Word format with standard legal formatting:

```
ARGUMENT

I. THE DISTRICT COURT ERRED IN GRANTING SUMMARY JUDGMENT

The Supreme Court has long held that summary judgment is inappropriate 
where genuine issues of material fact exist. Anderson v. Liberty Lobby, 
Inc., 477 U.S. 242, 248 (1986). As established in Celotex Corp. v. 
Catrett, 477 U.S. 317 (1986), the moving party bears the initial burden...
```

**Convert to text:**
1. In Word, go to File → Save As
2. Choose "Plain Text (.txt)" as the format
3. Name it "AppellateBrief_Draft.txt"
4. Click Save

#### 2. Upload and Analysis
1. Open the Citation Hallucination Detector in your browser
2. Enter your CourtListener API key
3. Click the upload area or drag your .txt file
4. Review the preview to ensure formatting looks correct
5. Click "Analyze Citations"

#### 3. Reviewing Results
The tool processes your brief and returns:

```
📊 Summary Statistics
Total Citations: 47
Verified: 42
Mismatches: 2
Possibly Hallucinated: 1
Not Found: 2
```

**Focus on Problems:**
The tool highlights five problematic citations:

**Citation #12: MISMATCH**
- Your text: "Smith v. Jones, 598 U.S. 112 (2019)"
- Database: "Johnson v. Texas, 598 U.S. 112 (2019)"
- Action: Replace "Smith v. Jones" with "Johnson v. Texas"

**Citation #28: NOT FOUND**
- Your text: "Williams v. State, 439 U.S. 891 (2018)"
- Action: This citation doesn't exist - verify and correct or remove

#### 4. Using Context Features
For each flagged citation, use the context inspector:
1. Adjust the before/after sliders to see more surrounding text
2. This helps you locate the exact position in your original document
3. Look for the highlighted citation within the context

#### 5. Exporting Results
Choose your preferred format for making corrections:

**Option A: Word/RTF Export (Recommended for briefs)**
1. Click "Word Compatible (RTF)"
2. Open the downloaded file in Word
3. You'll see an editing guide with each problem citation and its context
4. Use this alongside your original brief to make corrections

**Option B: PDF Export (For printing)**
1. Click "PDF Report"
2. Print the guide
3. Make handwritten notes for corrections
4. Apply changes to your original document

#### 6. Making Corrections
Using your export guide, return to your original Word document:

1. Use Ctrl+F (Find) to locate each problematic citation
2. For Citation #12: Find "Smith v. Jones, 598 U.S. 112" and replace with "Johnson v. Texas, 598 U.S. 112"
3. For Citation #28: Research the correct citation or remove the reference
4. Save your corrected document

#### 7. Verification Run
After corrections:
1. Convert the corrected document to .txt again
2. Run through the tool once more
3. Verify all citations now show as "Verified"

## Scenario 2: Checking a Law Review Article

### The Situation
You're a law review editor checking a submitted article that's 65 pages long with extensive footnotes. You need to verify citations efficiently while preserving academic formatting.

### Step-by-Step Process

#### 1. Handling Length Limitations
Since your article exceeds the 64,000 character limit:

**Split strategically:**
- Part 1: Introduction through Part II (pages 1-25)
- Part 2: Part III through Part IV (pages 26-50)  
- Part 3: Part V through Conclusion (pages 51-65)

**Extract text with footnotes:**
1. In Word, go to View → Draft view (this shows footnotes inline)
2. Select each section
3. Copy to separate text files
4. Name them: "LawReview_Part1.txt", "LawReview_Part2.txt", etc.

#### 2. Processing Each Section
Run each part through the tool separately:

**Part 1 Results:**
```
Total Citations: 89
Verified: 85
Issues: 4
```

**Common law review issues to watch for:**
- Shortened case names in footnotes
- Citations with parallel citations
- Historical cases with non-standard reporters

#### 3. Handling Special Situations

**Parallel Citations:**
The tool may flag legitimate parallel citations as "not found":
```
Your text: "Brown v. Board of Education, 347 U.S. 483, 74 S. Ct. 686, 98 L. Ed. 873 (1954)"
Tool result: Only recognizes "347 U.S. 483"
```
This is a limitation - manually verify these are correct.

**String Citations:**
```
See, e.g., Miranda v. Arizona, 384 U.S. 436 (1966); Gideon v. Wainwright, 
372 U.S. 335 (1963); Mapp v. Ohio, 367 U.S. 643 (1961).
```
The tool checks each citation individually - review them in sequence.

#### 4. Creating a Correction Spreadsheet
For law review work, export to CSV:

1. Click "CSV Data" for each part
2. Combine in Excel
3. Add columns for:
   - Footnote number
   - Author response needed (Y/N)
   - Editor notes
   - Verification status

#### 5. Author Communication
Create a revision memo using the Markdown export:

1. Click "Markdown Report"
2. Edit to include only problematic citations
3. Add specific instructions for the author
4. Include the context for each citation needing correction

## Understanding the Results

### Status Indicators

**✅ Verified**
- Citation exists and case name matches
- No action needed

**⚠️ Mismatch**
- Citation exists but with a different case name
- High priority for correction
- Often indicates copy-paste errors or hallucination

**❓ Possibly Hallucinated**
- Citation found but case name extraction failed
- Manually verify the case name

**❌ Not Found**
- Citation doesn't exist in the database
- Could be hallucinated, typo, or database limitation

### The Context Inspector

The context inspector helps you understand where citations appear:

```
Before: [100 chars] ← Adjust to see more preceding text
Citation: [highlighted]
After: [100 chars] ← Adjust to see more following text
```

**Green highlight**: Case name successfully extracted
**Yellow highlight**: The citation itself
**Blue highlight**: Detected case names in context

## Best Practices

### Before Running the Tool

1. **Clean your document**
   - Remove headers/footers
   - Eliminate page numbers
   - Keep footnotes with their text

2. **Preserve citation formatting**
   - Don't remove commas or periods
   - Keep parenthetical years
   - Maintain "v." or "vs." as written

3. **Check your limits**
   - Under 64,000 characters
   - Under 250 citations
   - If over, split strategically

### During Analysis

1. **Start with high-priority issues**
   - Fix mismatches first
   - Then address not-found citations
   - Finally review possibly hallucinated

2. **Use context wisely**
   - Expand context to find citations quickly
   - Look for signal phrases ("See", "accord", "cf.")
   - Note if citation is in a string cite

3. **Track your corrections**
   - Mark each fix in your export
   - Keep notes on ambiguous cases
   - Document any citations you're leaving as-is

### After Corrections

1. **Always re-run the check**
   - Verify your corrections worked
   - Catch any introduced errors
   - Ensure no citations were missed

2. **Manual verification still needed**
   - Check that cases support their propositions
   - Verify pinpoint citations
   - Ensure proper signal usage

## Troubleshooting

### Common Issues and Solutions

**"Rate limit exceeded" error**
- Wait 60 seconds before trying again
- Process documents one at a time
- Consider splitting large documents

**Citations not detected**
- Check for non-standard formatting
- Ensure spaces between reporter elements
- Verify parenthetical years are included

**False positives (valid citations marked as problems)**
- Historical cases may not be in database
- State court cases have limited coverage
- Recent cases may not be indexed yet

**Export not working**
- Check pop-up blocker settings
- Try a different export format
- Save the webpage as HTML as backup

### When to Trust vs. Verify

**High Confidence Scenarios:**
- Federal appellate cases (comprehensive coverage)
- Supreme Court cases (complete database)
- Recent federal district court cases

**Require Additional Verification:**
- State court cases (limited coverage)
- Historical cases (pre-1950)
- Specialized courts
- Very recent decisions (last 30 days)

## Advanced Tips

### Batch Processing
For multiple documents:
1. Prepare all as .txt files
2. Name systematically (Brief01.txt, Brief02.txt)
3. Process in sequence
4. Combine CSV exports in Excel

### Integration with Citation Managers
1. Export problematic citations as CSV
2. Import into Zotero/Mendeley
3. Verify against your citation library
4. Update your reference database

### Creating Templates
For repeated use:
1. Save your API key in browser
2. Create document prep checklist
3. Standardize export preferences
4. Build correction workflow

## Conclusion

The Legal Citation Hallucination Detector is a powerful first-line defense against citation errors, but it's not infallible. Use it as part of a comprehensive review process that includes:

1. Automated checking (this tool)
2. Manual verification of propositions
3. Shepardizing or KeyCiting
4. Peer review

Remember: The tool checks if citations exist and if case names match, but the responsibility for ensuring citations actually support their propositions remains with the legal professional.

For technical support and updates, check the repository documentation. For API issues, contact CourtListener support.