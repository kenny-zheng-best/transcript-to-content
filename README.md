# Turn Recordings into Posts

A Claude Code skill that transforms raw transcripts, conversations, and notes into ready-to-publish social media posts.

## What It Does

Takes long transcripts (recordings, conversations, meeting notes) and extracts:
- Counter-intuitive insights
- Complete logical arguments with data
- Standalone observations
- Deep analysis posts

**Learns your writing style** from your published content, so output matches your voice and platform conventions.

## Why This Exists

Every conversation, recording, or brainstorming session contains flashes of insight—ideas that only exist in that moment. Most of these insights vanish within hours:

- **Not captured**: We forget the best parts of our own conversations
- **Captured but buried**: Saved in notes no one will ever read again
- **Kept private**: Hidden insights that could help others facing similar challenges

This is a triple loss:

1. **Personal loss**: Your best thinking disappears
2. **Network loss**: People who could benefit never see it
3. **Influence loss**: Hidden knowledge doesn't build reputation

This skill solves all three:
- **Captures** tacit knowledge from recordings before it's lost
- **Transforms** raw thoughts into shareable, polished content
- **Amplifies** your ideas to reach the people who need them

**The deeper why**: Your conversations contain insights worth 100x more than most content online. But insights locked in your head (or buried in transcripts) create zero value. By turning recordings into shareable posts:
- You preserve hard-won knowledge
- Your network gains access to high-signal information
- You build a public track record of quality thinking

Sharing isn't just about "building influence"—it's about making your implicit knowledge explicit, so it can compound.

## Key Features

- **Style learning**: Analyzes 2-5 reference posts to match your tone, structure, and formatting
- **Smart extraction**: Analyzes content at 4 levels (short insights, observations, medium posts, deep analysis)
- **Rigorous fact-checking**: Verifies all proper nouns, data points, and logical connections
- **Mobile-optimized**: Short paragraphs, natural flow, compelling openings
- **Quality over quantity**: Strict 4 Iron Rules for insights (Independence, Completeness, Impact, Non-duplication)
- **Long transcript handling**: Uses index-retrieval strategy for 3000+ line transcripts to prevent fabrication

## Usage

```
/transcript-to-content
```

The skill will:
1. Ask for transcript file path (or you can provide it directly)
2. **Ask for reference style** (2-5 existing posts you've published) - HIGHLY RECOMMENDED
3. Analyze your style from references (structure, tone, formatting, platform conventions)
4. Build topic index for long transcripts
5. Extract and verify all content
6. Output all qualified content in your style, ready to publish

## Why Reference Content Matters

Providing 2-5 examples of your published content helps the skill:
- Match your paragraph length and structure
- Mirror your use of formatting (bold, lists, emojis)
- Adopt your opening style (direct, data-driven, storytelling, provocative)
- Match your tone (casual/formal, sharp/moderate)
- Understand your platform conventions (Twitter threads, LinkedIn posts, blog-style, etc.)

**Without references**: Skill uses neutral professional tone with clear structure.

**With references**: Output sounds like you wrote it yourself.

## Architecture

- **For long transcripts (>3000 lines)**: Uses Task tool with Explore agent to build comprehensive topic index, then Grep for spot verification per topic
- **For medium transcripts (1000-3000 lines)**: Chunked reading with targeted verification
- **For short transcripts (<1000 lines)**: Single-pass with light verification

## Quality Standards

### The 4 Iron Rules for Insights
1. **Independence**: Can be understood without context (max 1-2 sentences setup)
2. **Completeness**: Full meaning, not half a sentence
3. **Impact**: Data/contrast/counter-intuitive/emotional resonance
4. **Non-duplication**: Not already covered in a longer post

### Writing Principles
- **Delete until impossible** - Every sentence must advance the logic
- **Match user's style** - Learn from reference content if provided
- **No teaching tone** - Provide frameworks, don't preach
- **No anxiety farming** - Provide insights, don't manufacture panic
- **Have attitude** - Make clear judgments, don't hedge excessively

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

From a 5500-line transcript about AI companion products:
- Extracted 12 complete posts covering strategy, development, monetization
- Each post fact-checked against original transcript
- All data points verified (names, numbers, logical connections)
- Ready to publish with no additional editing

## Installation

This skill is designed for Claude Code. Place in your skills directory:

```
~/.claude/skills/transcript-to-content/
```

Or install via GitHub:

```bash
# Coming soon: one-command installation
```

## Use Cases

- **Podcast recordings** → Twitter threads, LinkedIn posts, blog posts
- **Meeting notes** → Shareable insights, team updates
- **Interview transcripts** → Quotable insights, key takeaways
- **Brainstorming sessions** → Polished ideas, strategic posts
- **Lecture recordings** → Educational content, explainer posts

## Platform Agnostic

Works with any social media platform or content format:
- Twitter/X (threads, standalone tweets)
- LinkedIn (professional posts)
- Blog posts (long-form analysis)
- Newsletter content
- Internal team communications
- Any format where you want to match your existing style

## License

MIT

## Author

Created for transforming long-form recordings into high-quality social media content that sounds like you.
