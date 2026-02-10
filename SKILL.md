---
name: transcript-to-content
description: Transform transcripts, conversations, or raw notes into batch social media content. Currently optimized for Chinese Jike (即刻) platform format. Handles transcripts of any length (tested up to 5500+ lines / 90+ min). Use when user: (1) provides transcript/conversation/recording and wants social content, (2) asks to "整理录音/对话/笔记成内容", (3) mentions generating posts from recordings/notes, (4) needs multiple publishable pieces from single source. Outputs 2-15 ready-to-publish pieces depending on content richness (insights 20-150 chars, posts 300-1000 chars).
---

# Transcript to Social Content Generator

Transform raw content (transcripts, conversations, notes) into 4-8 ready-to-publish Jike posts.

## Execution Workflow

### Step 1: Receive Input

Accept two inputs:
- **Required**: Raw content (transcript, conversation, notes)
- **Optional**: Reference style (2-5 existing Jike posts from user)

If user provides reference style, analyze and extract:
- Information density (data-heavy / case-heavy / opinion-heavy)
- Language style (formal / casual / sharp / moderate)
- **Structure observation** (CRITICAL):
  - Does it use bold subheadings? How many?
  - Does it use numbered lists (1. 2. 3.)?
  - Average paragraph length (1-2 lines / 3-4 lines / longer)?
  - Opening style (direct statement / data / story / question)?
- Attitude markers (explicit judgment / emotional words / teaching tone)

### Step 2: Extract All Possible Content

Identify content at 4 levels:

1. **Short insights (20-150 characters)** - Standalone observations or quotes
2. **Phenomenon posts (150-300 characters)** - Interesting observations with brief analysis
3. **Medium posts (300-600 characters)** - Complete logical argument with 3-4 supporting points
4. **Deep analysis (600-1000 characters)** - Complex multi-dimensional analysis

List all possibilities without filtering yet.

**⚠️ CRITICAL for long transcripts (>3000 lines):**

When dealing with very long transcripts, you MUST use a rigorous verification workflow to prevent errors:

1. **DO NOT rely on memory** - Don't write content from impression after reading the full transcript
2. **Extract and verify immediately:**
   - Use Grep to find relevant sections BEFORE writing each content piece
   - Read surrounding context (20-50 lines) to verify accuracy
   - Confirm exact wording, data points, and logical connections
3. **Common error patterns to avoid:**
   - **Fabricating data**: Don't add numbers/metrics that "feel right" but aren't in transcript
   - **False connections**: Don't connect two unrelated sections just because they seem related
   - **Over-generalization**: Don't change "最开始" to "所有" or "部分" to "全部"
   - **Misunderstanding context**: Don't create A→B logic when transcript only contains A and B separately
4. **Verification checklist for each piece:**
   - [ ] Grep searched the relevant section
   - [ ] Read 20-50 lines of context around the key points
   - [ ] All data points confirmed in original text
   - [ ] Logical connections verified (not assumed)
   - [ ] No generalizations beyond what transcript states

**Example of WRONG process:**
- Read full 5000-line transcript → Remember key points → Write all content from memory
- Result: Fabricated "Google Cloud 100刀/天", wrongly connected "30倍产能" with "脸刷到40岁"

**Example of CORRECT process:**
- Identify topic: "AI开发效率" → Grep for "30.*窗口|Claude.*账号" → Read lines 500-550 → Verify exact setup → Write content → Move to next topic

If uncertain about ANY detail, use Grep to re-verify before writing. Never guess or extrapolate.

**📏 Length-Based Processing Strategies:**

Based on first principles (memory is lossy compression, verification must be real-time), choose strategy by transcript length:

**1. Short transcripts (<1000 lines / <30 min):**
- **Strategy**: Single-pass with light verification
- **Workflow**:
  - Read full transcript once (fits comfortably in context)
  - Extract all potential content points while reading
  - Write content pieces from working memory
  - Verify only critical data points (numbers, names) using Grep
- **Why**: Short content can be held in working memory with minimal loss
- **Risk**: Low fabrication risk

**2. Medium transcripts (1000-3000 lines / 30-90 min):**
- **Strategy**: Chunked reading with targeted verification
- **Workflow**:
  - Read transcript in 2-3 logical chunks (by topic/section)
  - After each chunk, extract content points and write immediately
  - Use Grep to verify any uncertain details before finalizing each piece
  - Cross-reference between chunks if needed
- **Why**: Balance between speed and accuracy; chunks prevent memory overflow
- **Risk**: Medium - verify cross-chunk connections carefully

**3. Long transcripts (>3000 lines / >90 min):**
- **Strategy**: Two-pass index-retrieval (MANDATORY)
- **Workflow**:
  - **Pass 1 - Build index (15-20 min):**
    - **Option A (Recommended)**: Use Task tool with Explore agent (subagent_type=Explore, thoroughness="medium")
      - Agent will scan full transcript and build topic structure automatically
      - Returns organized list of topics with key points and approximate line ranges
      - Faster and more comprehensive than manual scanning
    - **Option B**: Quick scan with Grep to identify all major topics/keywords
      - Grep for high-level keywords to map topic distribution
      - Create sparse index: Topic → Keywords → Approximate line ranges
      - Example: "AI开发效率" → "Claude|窗口|账号" → lines 500-600, 1200-1300
  - **Pass 2 - Extract and verify immediately (per topic):**
    - For each topic in index:
      1. Grep search relevant keywords
      2. Read 20-50 lines of surrounding context
      3. Verify exact wording, data, logical connections
      4. Write content piece immediately
      5. Move to next topic (do NOT batch write later)
- **Why**: This is an index-retrieval problem, not full-text processing. Memory is unreliable beyond 3000 lines.
- **Risk**: High fabrication risk if you deviate from this workflow

**Key principle**: 记忆是有损压缩，验证必须实时进行 (Memory is lossy compression, verification must be real-time)

### Step 3: Deduplicate and Prioritize

**Deduplication rules:**
- If a point can expand into a full post, don't make it a standalone insight
- Use each point only once - don't repeat in both long post and short insight
- Same material can appear in different posts, but with different roles

**Prioritization criteria:**
- Hot topics (AI/startup/workplace) > niche topics
- Counter-intuitive insights > common observations
- Complete material > needs supplementing
- Specific data comparisons > abstract descriptions

**Determine output quantity based on content richness:**
- Rich content (90+ min, multiple topics): 10-15 total posts
  - Aim for comprehensive extraction - long transcripts contain many distinct angles
  - Don't stop at 6 posts if there are more quality topics to extract
- Medium content (30-90 min, 2-3 topics): 6-10 total posts
  - Extract multiple angles from each major topic
- Light content (<30 min, 1 topic): 2-4 total posts
  - Focus on depth rather than breadth

**Quality over arbitrary limits:**
- These are guidelines, not hard caps
- If user feedback suggests you extracted too few, go back and extract more angles
- Better to extract comprehensively upfront than in multiple rounds

**Output structure:**
- **High-priority content**: Output as "长文" - most impactful, counter-intuitive, or hot topics
- **Secondary content**: Output as "其他可发布内容" - still quality-checked but slightly lower priority/impact
- **IMPORTANT**: Write FULL content for secondary posts, not just outline - let user decide what to publish

**Insights (金句) quantity is NOT predetermined:**
- Apply strict 4 Iron Rules filtering to ALL candidates
- Output as many as pass the standard (could be 0, 1, 5, or 10)
- DO NOT artificially limit quantity if quality is met
- DO NOT force output if quality is not met
- Quality standard is fixed, quantity is the natural result

If content is too personal or lacks universal value, tell user honestly.

### Step 4: Write Content

**For each piece:**

1. **Choose appropriate length** based on topic complexity

2. **Jike Platform Characteristics (CRITICAL - 即刻不是公众号):**
   - Feed流阅读，不是深度阅读 - first 3 lines determine if user expands
   - Structure must be INVISIBLE - logic flows naturally, not forced into sections
   - ⚠️ **AVOID over-structuring:**
     - NO excessive bold subheadings (**小标题**)
     - NO numbered lists (1. 2. 3.) unless user's style uses them
     - NO transition phrases ("接下来", "总结一下", "综上所述")
   - Short paragraphs - max 3-4 lines per paragraph
   - **CRITICAL: Add blank line between paragraphs** for better reading experience on Jike
   - Data must grab attention in first few lines

3. **"删无可删" Execution (Delete Until Impossible):**

   **First pass - Delete structural fluff:**
   - Delete bold formatting on subheadings (unless user's style uses them)
   - Delete numbered list markers
   - Delete transition sentences between sections
   - Delete summary conclusions at the end

   **Second pass - Test each sentence:**
   - Ask: "If I delete this sentence, does the logic break?"
   - If logic doesn't break → DELETE
   - If it's just repeating the previous point → DELETE
   - If it's an obvious statement everyone knows → DELETE

   **Third pass - Check structure:**
   - Does structure feel natural or forced?
   - If it reads like a PowerPoint → REWRITE without visible structure
   - Logic should flow through content, not through formatting

4. **Follow core principles:**
   - One theme per piece - don't try to cover everything
   - Logic > case studies - use cases only as evidence
   - 1000 character max (Jike platform limit)
   - Match user's style if reference provided
   - Have attitude - make clear judgments, don't hedge

5. **For short insights (STRICT FILTERING):**
   - **4 Iron Rules - ALL must pass:**
     - ✅ Independence: Stranger can understand (max 1-2 sentences context)
     - ✅ Completeness: Full meaning, not half a sentence
     - ✅ Impact: Data/contrast/counter-intuitive/emotional resonance
     - ✅ Non-duplication: Not already in a long post
   - **宁缺毋滥原则:**
     - Apply standard strictly to ALL candidates
     - Output every insight that passes (no upper limit)
     - Output zero if none pass (no lower limit)
     - Quality standard is fixed, quantity is natural result
   - If none pass all 4 tests → Output "No suitable standalone insights found"
   - See [references/golden-sentence-standards.md](references/golden-sentence-standards.md) for examples

6. **Check against quality standards:**
   - See [references/writing-checklist.md](references/writing-checklist.md) for complete checklist

7. **Fact check (CRITICAL):**
   - Transcripts often contain errors (mishearing, wrong names, incorrect data)
   - **For ALL proper nouns (people names, company names, product names):**
     - Search to verify correct spelling and accuracy
     - Example: "墨子号" might be misheard, verify the actual person's name
     - Example: "Luki" vs "Looki" - check correct product name
   - **For ALL data points (revenue, valuation, dates):**
     - If transcript says "Plaud 3亿美金", search to confirm
     - If uncertain, use hedge language or remove the data
   - **When to search:**
     - Any name that seems unusual or might be misheard
     - Any surprising data that could be wrong
     - Any product/company you're not 100% certain about
   - **⚠️ For long transcripts, verify EVERYTHING:**
     - Don't assume you remember correctly - Grep and re-read the section
     - Even seemingly "obvious" facts should be verified (e.g., "小龙虾1亿美金收购" - is this actually in the transcript?)
     - Check logical connections - are two points really related, or did you connect them in your mind?
   - Better to remove questionable details than publish incorrect information

### Step 5: Batch Output

Output format:

```
从你的录音稿中提取了 X 条可发布内容，按质量排序：

---

## 内容 1 | 标题

[完整内容 - 干净可发布]

---

## 内容 2 | 标题

[完整内容]

---

## 内容 3 | 标题

[完整内容]

---

## 内容 4 | 标题

[完整内容]

---

[继续输出所有内容...]
```

**IMPORTANT:**
- NO category headers (【长文】/【金句】/【其他可发布内容】)
- Sort ALL content by quality (highest first), regardless of type (long post / insight / secondary)
- Clean markdown only - each piece separated by `---`
- Each piece header format: `## 内容 X | 标题` (concise title summarizing the topic)
- DO NOT include meta-information (priority, engagement, recommendations)
- Output should be CLEAN and READY-TO-PUBLISH

After output, ask: "满意吗？需要调整哪些内容？"

## Common Adjustments

If user says:
- "Too long" → Condense to target length
- "Can't understand the insight" → Add simple background
- "Wrong style" → Adjust tone
- "Want more" → Write expandable topics
- "Not enough insights" → Find more (but maintain quality standards)

## Key Principles

1. **Always batch output** - Write all qualified content upfront (including secondary posts), don't ask which to write first
2. **Quality > quantity** - Don't force output if material is weak
3. **Clean output only** - No meta-information (priority stars, recommendations, predictions) in final output
4. **Fact check everything** - Verify names, data, and facts before outputting (transcripts have errors)
5. **即刻 ≠ 公众号** - Structure must be invisible, logic flows naturally, avoid over-formatting
6. **Strict insight filtering** - 宁缺毋滥, quality standard is fixed, quantity is natural result (see [golden-sentence-standards.md](references/golden-sentence-standards.md))
7. **删无可删** - Every sentence must advance the logic, delete everything else
8. **No teaching tone** - Provide frameworks, don't preach with "建议/应该"
9. **No anxiety farming** - Provide insights, don't manufacture panic
10. **Have attitude** - Make clear judgments, don't hedge excessively

## Reference Files

When in doubt about:
- **Insight quality**: Read [references/golden-sentence-standards.md](references/golden-sentence-standards.md)
- **Writing standards**: Read [references/writing-checklist.md](references/writing-checklist.md)

## Technical Troubleshooting

**Reading files with special characters in filename:**
- If filename contains quotes ("), colons (:), or other special characters
- Use bash workaround: `cd "directory" && cat "$(ls | grep 'partial_name' | head -1)"`
- Example: File named `AI陪伴产品战略：借力"酒馆"生态.md`
  - Don't use Read tool directly with full path (will fail)
  - Use: `cd "/path/to/dir/" && cat "$(ls | grep 'AI陪伴' | head -1)"`

**Handling large output files:**
- If Bash output exceeds ~100KB, it will be saved to persisted-output file
- Tool result will show: "Output too large (XXX KB). Full output saved to: /path/to/file"
- Use Read tool to read the persisted-output file path
- This commonly happens with long transcripts (>3000 lines)

**File not found errors:**
- If Read tool fails with "File does not exist" but Glob shows the file exists
- Likely cause: special characters in path or filename
- Solution: Use Bash with proper quoting or the ls workaround above

**Memory management for long transcripts:**
- Don't try to load entire 5000+ line transcript into single context
- Use index-retrieval strategy (Task tool for indexing + Grep for spot verification)
- Process topic-by-topic rather than trying to remember everything
