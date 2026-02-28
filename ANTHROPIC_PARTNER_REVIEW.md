# Anthropic Partner Plugin Review - BetterCallClaude v3.1.0

**Review Date**: 2026-02-28
**Reviewer**: Automated codebase audit
**Plugin Version**: 3.1.0
**License**: AGPL-3.0

---

## Executive Summary

**Verdict: READY FOR SUBMISSION**

BetterCallClaude is a Swiss Legal Intelligence plugin comprising 18 agents, 17 commands, 10 skills, and 5 MCP servers. The plugin is well-architected with strong security practices, comprehensive privacy protection (attorney-client privilege hooks), proper MCP spec compliance, and professional documentation.

All critical issues identified during the review have been resolved:
- Version inconsistencies fixed across all package manifests
- MCP SDK version unified to ^1.26.0 across all workspaces
- All npm audit vulnerabilities resolved (0 remaining)
- Root package-lock.json generated for reproducible builds
- Repository URLs corrected in MCPB manifests

---

## 1. Plugin Structure

### Components Verified

| Component | Count | Status |
|-----------|-------|--------|
| Agents | 18 | Verified |
| Commands | 17 | Verified |
| Skills | 10 | Verified |
| MCP Servers | 5 | Verified |
| Hooks | 1 (privacy-check) | Verified |

### Configuration Files

| File | Status | Notes |
|------|--------|-------|
| bettercallclaude/.claude-plugin/plugin.json | Valid | v3.1.0, correct structure |
| bettercallclaude/.claude-plugin/.mcp.json | Valid | 5 server entries, stdio transport |
| bettercallclaude/hooks/hooks.json | Valid | Pre-tool privacy hook |
| mcpb/manifests/*.json | Fixed | Updated to v3.1.0, corrected repo URLs |

### MCP Servers

| Server | Transport | Tools | SDK Version |
|--------|-----------|-------|-------------|
| bge-search | stdio | 3 | ^1.26.0 |
| entscheidsuche | stdio | 7 | ^1.26.0 |
| fedlex-sparql | stdio | 5 | ^1.26.0 |
| legal-citations | stdio | 8 | ^1.26.0 |
| onlinekommentar | stdio | 4 | ^1.26.0 |

---

## 2. Security Audit

### Credentials and Secrets

| Check | Result |
|-------|--------|
| Hardcoded API keys | None found |
| Hardcoded passwords | None found |
| Bearer tokens | None found |
| Private keys | None found |
| .env files committed | None |
| Credential patterns in source | Clean |

All sensitive configuration uses process.env environment variables. The only "secret" terminology found relates to Swiss legal professional secrecy concepts (Anwaltsgeheimnis/attorney-client privilege), which is appropriate domain language.

### Code Safety

| Check | Result |
|-------|--------|
| Dangerous code patterns | None |
| child_process / spawn / fork | None |
| Dynamic code generation | None |
| Shell command injection vectors | None |

### Data Handling

| Check | Result |
|-------|--------|
| Telemetry / analytics | None |
| Data collection | None |
| External data transmission | Only to public Swiss government APIs |
| User data storage | Local SQLite cache only |
| PII handling | Privacy hook intercepts privileged content |

### Input Validation

| Check | Result |
|-------|--------|
| SQL injection protection | Parameterized queries via TypeORM |
| SPARQL injection protection | Parameterized query templates |
| Citation format validation | Regex-based validation |
| API input sanitization | Joi schema validation |

### Privacy Protection (Anwaltsgeheimnis Hook)

The plugin includes a pre-tool hook (bettercallclaude/scripts/privacy-check.sh) that scans for attorney-client privilege patterns in DE/FR/IT before allowing tool use. When privilege indicators are detected, the hook returns an "ask" decision, requiring user confirmation before proceeding.

---

## 3. Dependency Audit

### npm Audit Results

```
mcp-servers-src/: found 0 vulnerabilities
root/: found 0 vulnerabilities
```

### Key Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| @modelcontextprotocol/sdk | ^1.26.0 | MCP protocol implementation |
| typeorm | ^0.3.19 | Database ORM |
| axios | ^1.6.0 | HTTP client |
| joi | ^17.11.0 | Input validation |
| winston | ^3.11.0 | Structured logging |
| natural | ^6.10.0 | NLP processing |
| sql.js | ^1.11.0 | SQLite (WASM) |

### DevDependencies (Post-Fix)

| Package | Version | Notes |
|---------|---------|-------|
| @typescript-eslint/* | ^8.56.1 | Upgraded from v6 (resolved ReDoS) |
| eslint | ^9.39.3 | Upgraded from v8 |
| esbuild | ^0.27.3 | Upgraded (resolved path traversal) |
| typescript | ^5.7.2 | Current |
| vitest | ^4.0.15 | Current |

---

## 4. Code Quality

### TypeScript Configuration

| Setting | Value | Assessment |
|---------|-------|------------|
| strict | true | Excellent |
| noUnusedLocals | true | Good |
| noImplicitReturns | true | Good |
| target | ES2022 | Appropriate for Node 18+ |

### Code Patterns

- All MCP servers follow consistent architecture: tool registration, input validation, error handling
- Shared infrastructure (@bettercallclaude/shared) provides reusable database, API client, and validation modules
- Error responses follow MCP error format with proper error codes
- No console.log in production paths (some console.error exists for startup/fatal errors)

---

## 5. MCP Compliance

### Protocol Adherence

| Requirement | Status |
|-------------|--------|
| Uses @modelcontextprotocol/sdk | Yes |
| stdio transport | Yes (all 5 servers) |
| Tool schemas with inputSchema | Yes |
| Proper error responses | Yes |
| Server info (name, version) | Yes |
| Tool descriptions | Yes (clear, actionable) |

### Tool Schema Quality

All tools provide:
- Clear description fields explaining purpose and usage
- JSON Schema inputSchema with required/optional properties
- Type annotations for all parameters
- Meaningful error messages on invalid input

---

## 6. Documentation

### README.md

- 487 lines, comprehensive
- Installation instructions for Claude Code CLI and Cowork Desktop
- Full component listing (agents, commands, skills, MCP servers)
- Architecture overview with clear diagrams
- Contributing guidelines
- License information (AGPL-3.0)

### Additional Documentation

| File | Purpose | Status |
|------|---------|--------|
| CONNECTORS.md | MCP server API documentation | Complete |
| CHANGELOG.md | Version history | Current (v3.1.0) |
| docs/installation-guide.md | Detailed setup instructions | Complete |

### Legal Disclaimers

Present in:
- README.md (general disclaimer)
- 6 of 10 skills (domain-specific disclaimers)
- Agent definitions (analysis output disclaimers)

All disclaimers clearly state output does not constitute legal advice and requires professional review.

---

## 7. Compliance Checklist

| # | Criterion | Status | Notes |
|---|-----------|--------|-------|
| 1 | Valid plugin.json | Pass | Correct schema, version 3.1.0 |
| 2 | No hardcoded secrets | Pass | All via env vars |
| 3 | No telemetry/data collection | Pass | No tracking code |
| 4 | Proper MCP SDK usage | Pass | ^1.26.0 unified |
| 5 | stdio transport | Pass | All 5 servers |
| 6 | Tool schemas valid | Pass | JSON Schema with types |
| 7 | Error handling | Pass | MCP error format |
| 8 | Input validation | Pass | Parameterized queries, Joi |
| 9 | No dangerous code patterns | Pass | Clean codebase |
| 10 | Documentation complete | Pass | README, CONNECTORS, CHANGELOG |
| 11 | License declared | Pass | AGPL-3.0 |
| 12 | Version consistency | Pass | 3.1.0 everywhere (fixed) |
| 13 | Dependencies secure | Pass | 0 npm audit vulnerabilities |
| 14 | Privacy protection | Pass | Anwaltsgeheimnis hook |
| 15 | Professional disclaimers | Pass | Legal advice disclaimers present |
| 16 | Repository URL correct | Pass | fedec65/BetterCallClaude (fixed) |
| 17 | Lockfile present | Pass | package-lock.json at root |
| 18 | TypeScript strict mode | Pass | strict: true |

---

## 8. Issues Resolved During Review

### Critical (Fixed)

1. **Version inconsistency**: mcp-servers-src/package.json was 3.0.0, now 3.1.0
2. **MCPB manifest versions**: All 5 manifests updated from 3.0.0 to 3.1.0
3. **MCPB repository URLs**: Changed from deprecated BetterCallClaude_Marketplace to BetterCallClaude
4. **MCP SDK mismatch**: 6 workspace package.json files updated from ^1.24.0 to ^1.26.0

### Recommended (Fixed)

5. **npm audit vulnerabilities**: Upgraded @typescript-eslint from v6 to v8, eslint from v8 to v9, esbuild from v0.24 to v0.27. Result: 0 vulnerabilities
6. **Missing root lockfile**: Generated package-lock.json at repository root

### Minor (Noted for Future)

7. **SECURITY.md**: Consider adding a vulnerability reporting process document
8. **Bundle sizes**: bge-search and entscheidsuche bundles are approximately 24MB due to SQLite+pg bundling. Acceptable but could optimize with tree-shaking
9. **Test coverage**: Tests exist and are meaningful but no --coverage flag configured in CI
10. **Structured logging**: 22 instances of console.error across MCP servers could use Winston logger for consistency

---

## Conclusion

BetterCallClaude v3.1.0 meets all requirements for Anthropic's official plugin directory. The plugin demonstrates:

- **Security excellence**: No hardcoded credentials, no unsafe code patterns, parameterized queries throughout
- **Privacy-first design**: Attorney-client privilege protection via pre-tool hooks
- **MCP compliance**: All servers follow the protocol specification correctly
- **Professional quality**: Strict TypeScript, comprehensive documentation, proper error handling
- **Domain expertise**: Purpose-built for Swiss legal professionals with appropriate disclaimers

The plugin is ready for submission.
