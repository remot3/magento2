# Security Review Guidelines

This repository is a Magento PHP codebase without vendor dependencies checked in. To optimize security review:

1. **Install dependencies** (if allowed):
   ```bash
   composer install --no-interaction
   ```
   This installs tooling such as `phpstan`, `phpcs`, and `phpmd`.

2. **Static analysis commands** (run from repository root):
   ```bash
   vendor/bin/phpstan analyse -c dev/tests/static/testsuite/Magento/Test/Php/_files/phpstan/phpstan.neon
   vendor/bin/phpcs -n --standard=Magento2 app lib
   vendor/bin/phpmd app,lib text cleancode,codesize,unusedcode,naming
   ```
   These commands highlight code quality and potential security issues. If they fail due to missing packages or network limits, note that in your testing summary.

3. **Search for dangerous sinks.** Use `grep` or `ripgrep` to scan for risky functions:
   ```bash
   grep -R "\b(exec|shell_exec|system|passthru|popen|proc_open|eval|assert)\b" -n
   grep -R "\binclude\b\s*\(" -n
   grep -R "->query(\"" -n
   ```
   Review any matches carefully. Pay particular attention to code that ignores linting rules with `phpcs:ignore` or `phpstan-ignore` comments.

4. **Document findings**. Summarize any security concerns, noting file paths and line numbers.

These steps help uncover dangerous code sinks and maintain secure coding standards.
