# Legal Citation Hallucination Detector

A web-based tool for detecting hallucinated legal citations in documents, powered by CourtListener's Eyecite API.

## ⚠️ IMPORTANT LIMITATIONS AND WARNINGS

### This Tool is Both Overinclusive and Underinclusive

- **Overinclusive**: May flag valid citations as potential hallucinations (false positives)
- **Underinclusive**: May miss some hallucinated citations (false negatives)
- **Legal hallucination parsing is an inherently difficult task** - Use this tool as an aid, not as definitive proof

### ⚠️ CRITICAL: No Relevance or Proposition Checking

**This tool does NOT verify whether a case actually supports the proposition for which it is cited.**

For example, the following nonsensical statement would pass verification:
> "Obergefell v. Hodges, 576 U.S. 644 (2015), held that the president lacks power to veto laws on Tuesdays."

The tool would mark this as ✅ VERIFIED because:
- The citation format is correct
- The case exists at 576 U.S. 644
- The case name matches the database

**What the tool completely misses:**
- Obergefell v. Hodges is about same-sex marriage rights, not presidential veto power
- The cited holding is completely fabricated
- The case has no relevance to the stated proposition

This is a fundamental limitation: **the tool only checks if citations exist and match their case names, NOT whether they support their claimed propositions.**

### API Rate Limits

1. **Citation Throttle**: Maximum 60 valid citations per minute
   - Exceeding this limit results in HTTP 429 error
   - Response includes `wait_until` ISO-8601 timestamp for next allowed request

2. **Per-Request Limit**: Maximum 250 citations per single request
   - Citations beyond #250 are parsed but not matched
   - These citations receive status 429 ("Too many citations requested")

3. **Text Size Limit**: Maximum 64,000 characters per request
   - Approximately 50 pages of legal content
   - Larger requests blocked for security

4. **General Rate Limiting**: Standard CourtListener API throttle applies
   - Prevents denial of service attacks
   - Most users will never encounter this limit

### Citation Type Limitations

The following citation types are **NOT supported**:
- ❌ Statutes
- ❌ Law journal citations
- ❌ "Id." references
- ❌ "Supra" references
- ❌ Citations without volume numbers (e.g., "22 U.S. ___")
- ❌ Citations without page numbers

To match these citation types, use [Eyecite](https://github.com/freelawproject/eyecite) directly.

### Detection Accuracy

1. **Case Name Extraction**: May fail to extract case names from certain formatting styles
2. **Partial Matches**: May incorrectly flag abbreviated case names as mismatches
3. **Context Dependency**: Accuracy depends on having sufficient context around citations
4. **OCR/PDF Issues**: PDF and DOCX extraction is basic and may miss or misread citations

## What This Tool Does Well

✅ **Type 1 Hallucinations**: Detects when a real citation (e.g., "410 U.S. 113") is attributed to the wrong case name  
✅ **Type 3 Hallucinations**: Identifies completely fictional citations  
✅ **Actionable Export**: Provides specific editing instructions with context markers  
✅ **Bulk Processing**: Analyzes multiple citations in a single document  

## What This Tool Does NOT Do Well

❌ **Relevance Checking**: Cannot determine if a case supports the proposition for which it's cited  
❌ **Substantive Analysis**: Cannot detect when accurate citations are used to support false claims  
❌ **Complex Citations**: Cannot parse statutes, regulations, or secondary sources  
❌ **Cross-References**: Cannot resolve "id." or "supra" references  
❌ **Incomplete Citations**: Cannot verify citations missing volume/page numbers  
❌ **Perfect Accuracy**: Both false positives and false negatives are common  

## How to Use

1. **Upload** a legal document (.txt, .md, .pdf, or .docx)
2. **Enter** your CourtListener API key (get free at [courtlistener.com](https://www.courtlistener.com/sign-in/))
3. **Analyze** the citations found
4. **Export** actionable editing guides in various formats:
   - JSON: Structured data with context markers
   - Word/RTF: Human-readable editing instructions
   - PDF: Print-friendly guide with highlighted issues
   - CSV: Spreadsheet format for bulk analysis

## Best Practices

1. **Manual Review Required**: Always manually verify flagged citations
2. **Use Context**: Review the surrounding text, not just the citation
3. **Check Original Sources**: When in doubt, verify against official reporters
4. **Understand Limitations**: This tool is an aid, not a replacement for legal research

## Technical Details

- **API**: CourtListener Citation Lookup API v3
- **Parser**: Eyecite citation parser
- **Browser Compatibility**: Modern browsers (Chrome, Firefox, Safari, Edge)
- **Privacy**: All processing happens via API calls; no data is stored

## Getting Your API Key

1. Visit [CourtListener Sign In](https://www.courtlistener.com/sign-in/)
2. Create a free account
3. Navigate to your profile settings
4. Generate an API token
5. Copy and paste into the tool

## Support and Feedback

This tool is provided as-is for educational and research purposes. For issues with:
- **Citation parsing**: See [Eyecite documentation](https://github.com/freelawproject/eyecite)
- **API issues**: Contact [CourtListener support](https://www.courtlistener.com/contact/)
- **Tool bugs**: Open an issue in this repository

## Disclaimer

- **This tool is NOT intended to provide legal advice**
- For educational and research purposes only
- Use at your own risk
- The authors assume no liability for decisions made based on this tool's output
- Always consult qualified legal professionals for legal matters

## Acknowledgments

- [Free Law Project](https://free.law/) for CourtListener and Eyecite
- Legal technology community for ongoing feedback and improvements