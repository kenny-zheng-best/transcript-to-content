# Transcript to Social Content Generator

A Claude Code skill that transforms raw transcripts, conversations, and notes into 4-8 ready-to-publish Jike (即刻) posts.

## What It Does

Takes long transcripts (recordings, conversations, meeting notes) and extracts:
- Counter-intuitive insights
- Complete logical arguments with data
- Standalone observations
- Deep analysis posts

Optimized for Chinese social media platform Jike, with strict quality filtering and fact-checking built in.

## Key Features

- **Smart extraction**: Analyzes content at 4 levels (short insights, observations, medium posts, deep analysis)
- **Rigorous fact-checking**: Verifies all proper nouns, data points, and logical connections
- **Jike-optimized**: Invisible structure, natural flow, short paragraphs, mobile-first reading
- **Quality over quantity**: Strict 4 Iron Rules for insights (Independence, Completeness, Impact, Non-duplication)
- **Long transcript handling**: Uses index-retrieval strategy for 3000+ line transcripts to prevent fabrication

## Usage

```
/transcript-to-content
```

The skill will:
1. Ask for transcript file path (or you can provide it directly)
2. Optionally ask for reference style (2-5 existing posts)
3. Analyze transcript and build topic index
4. Extract and verify all content
5. Output all qualified content in clean, ready-to-publish format

## Architecture

- **For long transcripts (>3000 lines)**: Uses Task tool with Explore agent to build comprehensive topic index, then Grep for spot verification per topic
- **For medium transcripts (1000-3000 lines)**: Chunked reading with targeted verification
- **For short transcripts (<1000 lines)**: Single-pass with light verification

## Quality Standards

### The 4 Iron Rules for Insights
1. **Independence**: Can be understood by strangers (max 1-2 sentences context)
2. **Completeness**: Full meaning, not half a sentence
3. **Impact**: Data/contrast/counter-intuitive/emotional resonance
4. **Non-duplication**: Not already covered in a long post

### Writing Principles
- 删无可删 (Delete until impossible) - Every sentence must advance the logic
- 即刻 ≠ 公众号 - Structure must be invisible, not forced into sections
- No teaching tone - Provide frameworks, don't preach
- No anxiety farming - Provide insights, don't manufacture panic
- Have attitude - Make clear judgments, don't hedge excessively

## File Structure

```
transcript-to-content/
├── SKILL.md                              # Main skill prompt
├── README.md                             # This file
└── references/
    ├── golden-sentence-standards.md      # Quality standards for insights
    └── writing-checklist.md              # Complete writing checklist
```

## Technical Details

- Handles transcripts with special characters in filenames
- Manages large output files (persisted-output for >100KB)
- Supports batch parallel tool calls for verification
- Uses Explore agent for comprehensive topic indexing

## Example Output

From a 5500-line transcript about AI companion products, extracted:
- 12 complete posts covering strategy, development, monetization
- Each post fact-checked against original transcript
- All data points verified (names, numbers, logical connections)
- Ready to publish with no additional editing

## Installation

This skill is designed for Claude Code. Place in your skills directory:

```
~/.claude/skills/transcript-to-content/
```

## License

MIT

## Author

Created for transforming long-form recordings into high-quality social media content.
