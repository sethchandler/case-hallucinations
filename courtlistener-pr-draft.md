# Enhancement: Improve Citation Hallucination Detection in Citation Lookup API

## Problem Statement

The current Citation Lookup API has a significant gap in detecting Type 1 hallucinations - citations where the reporter citation (e.g., "410 U.S. 113") exists in the database but is attributed to an incorrect case name in the source text.

### Current Behavior
When analyzing text containing `"Roe v. Wade, 411 U.S. 121 (1973)"`:
- API returns `status: 200` (Found)
- API returns the correct case at that citation: "San Antonio Independent School District v. Rodriguez"
- **Issue**: No indication that the claimed case name "Roe v. Wade" doesn't match the actual case

This creates a false sense of security for users checking AI-generated legal content, as the most subtle and dangerous type of hallucination goes undetected.

## Types of Citation Hallucinations

1. **Type 1 (Undetected)**: Real citation, wrong case name
   - Input: `"Lemming v. Jacobs, 410 U.S. 113 (1973)"`
   - Reality: 410 U.S. 113 is actually "Roe v. Wade"
   - Current API: Returns 200 + correct case info
   - Problem: No mismatch warning

2. **Type 2 (Partially detected)**: Real case name, wrong citation
   - Input: `"Roe v. Wade, 411 U.S. 121 (1973)"`
   - Reality: Different case exists at 411 U.S. 121
   - Current API: Returns 200 + wrong case OR 404
   - Problem: No explicit mismatch detection

3. **Type 3 (Properly detected)**: Completely fictional
   - Input: `"Jones v. Fakename, 900 U.S. 422 (2027)"`
   - Current API: Returns 404 (Not Found) ✓

## Proposed Solution

### Option 1: Enhanced Response Format (Recommended)
Add case name verification to the existing API response:

```json
{
  "citation": "Roe v. Wade, 411 U.S. 121 (1973)",
  "normalized_citations": ["411 U.S. 121"],
  "status": 200,
  "case_name_status": "mismatch",  // NEW FIELD
  "extracted_case_name": "Roe v. Wade",  // NEW FIELD
  "clusters": [{
    "case_name": "San Antonio Independent School District v. Rodriguez",
    // ... existing fields
  }],
  "start_index": 125,
  "end_index": 155
}
```

New `case_name_status` values:
- `"verified"` - Case name matches database
- `"mismatch"` - Citation exists but case name doesn't match
- `"not_extracted"` - Could not extract case name from text
- `"not_applicable"` - Citation not found (status != 200)

### Option 2: Separate Verification Endpoint
Create a new endpoint `/api/rest/v4/citation-verify/` that specifically focuses on hallucination detection:

```json
POST /api/rest/v4/citation-verify/
{
  "text": "As stated in Roe v. Wade, 411 U.S. 121 (1973)..."
}

Response:
{
  "citations": [{
    "original_text": "Roe v. Wade, 411 U.S. 121 (1973)",
    "claimed_case": "Roe v. Wade",
    "actual_case": "San Antonio Independent School District v. Rodriguez",
    "hallucination_type": "case_name_mismatch",
    "confidence": 0.95,
    "citation_exists": true,
    "case_name_matches": false
  }]
}
```

## Implementation Suggestions

### Case Name Extraction
The API already uses eyecite for citation parsing. Enhance it to:

1. **Extract case names** from citation text using patterns like:
   - `/^([^,\d]+?)\s*,?\s*\d+/` - Everything before first number
   - `/^([^(]+?)\s*\(/` - Everything before parenthesis  
   - Handle variations: "v.", "vs.", "et al.", etc.

2. **Normalize case names** for comparison:
   - Convert to lowercase
   - Standardize "v." vs "vs." 
   - Remove extra whitespace
   - Handle common abbreviations

3. **Fuzzy matching** for partial matches:
   - Account for shortened case names
   - Handle plaintiff/defendant variations
   - Use similarity scoring (70%+ word overlap)

### Database Integration
Leverage existing case name data in CourtListener's database:
- Compare extracted case names against `case_name` field in clusters
- Use existing full-text search capabilities
- Consider alternative case name formats and abbreviations

## Benefits

1. **Improved AI Safety**: Detect the most dangerous type of legal hallucination
2. **User Confidence**: Clear indication when citations are suspicious
3. **Backward Compatible**: New fields don't break existing implementations
4. **Performance**: Minimal additional processing using existing eyecite infrastructure
5. **Research Value**: Data on hallucination patterns in legal AI systems

## Use Cases

- **Legal AI Validation**: Law firms checking AI-generated briefs
- **Academic Research**: Studying citation accuracy in large document sets  
- **Compliance**: Meeting accuracy requirements for legal technology
- **Education**: Teaching proper citation verification techniques

## Testing Strategy

Include test cases for each hallucination type:

```python
def test_case_name_mismatch():
    response = citation_lookup("Lemming v. Jacobs, 410 U.S. 113 (1973)")
    assert response['case_name_status'] == 'mismatch'
    assert response['extracted_case_name'] == 'Lemming v. Jacobs'
    assert 'Roe v. Wade' in response['clusters'][0]['case_name']

def test_case_name_verified():
    response = citation_lookup("Roe v. Wade, 410 U.S. 113 (1973)")
    assert response['case_name_status'] == 'verified'
```

## Alternative: Client-Side Implementation Note

While we've implemented a workaround using span data to extract original text and perform case name comparison client-side, this approach has limitations:
- Requires additional API calls or complex text processing
- Less accurate than server-side implementation with full eyecite capabilities
- Duplicates effort across different client implementations
- Cannot leverage CourtListener's comprehensive case name database

A server-side solution would be more reliable, performant, and beneficial to the entire legal technology community.

---

## Impact

This enhancement would make CourtListener's Citation Lookup API the gold standard for detecting AI hallucinations in legal citations, providing crucial functionality for the growing field of legal AI safety and validation.

The legal profession is increasingly relying on AI tools, and accurate citation verification is essential for maintaining the integrity of legal documents and court filings. This improvement would directly address a critical gap in current AI safety tools for legal practice.