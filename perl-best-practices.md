# Perl Best Practices

Perl can be expressive without becoming obscure. Production Perl should be
strict, readable, explicit about failure, and easy to run from the command line.

## Core Principles

1. Prefer clear code over clever syntax.
2. Make input, output, and error behavior predictable.
3. Keep scripts composable.
4. Use the standard library and trusted modules before custom code.
5. Avoid magic that hides intent from reviewers.

## Required Pragmas

1. Enable strictness and warnings in every script and module.

```perl
use strict;
use warnings;
```

2. Use a minimum Perl version when the project depends on newer behavior.

```perl
use v5.32;
```

3. Prefer lexical variables with `my`.
4. Avoid package globals unless the value truly belongs to the package.

## Naming

1. Use descriptive names for scalars, arrays, hashes, and subroutines.
2. Avoid one-letter names except for tiny local loops.
3. Name boolean values as questions or states.
4. Keep naming consistent across related scripts.

```perl
my $retry_count = 3;
my %users_by_email;
my $has_valid_signature = verify_signature($payload);
```

## Subroutines

1. Keep subroutines focused on one task.
2. Validate arguments near the top of the subroutine.
3. Return explicit values.
4. Avoid relying on implicit `$_` in non-trivial code.
5. Prefer named intermediate values over dense chains.

```perl
sub normalize_email {
    my ($email) = @_;

    die 'email is required' if !defined $email;

    $email =~ s/^\s+|\s+$//g;
    return lc $email;
}
```

## Files And IO

1. Use three-argument `open`.
2. Use lexical filehandles.
3. Check file operations.
4. Include useful error messages with `$!`.
5. Specify encodings for text files.

```perl
open my $fh, '<:encoding(UTF-8)', $path
    or die "Cannot open $path: $!";

while (my $line = <$fh>) {
    chomp $line;
    process_line($line);
}
```

## Regular Expressions

1. Keep regular expressions readable.
2. Use named captures for important values.
3. Use `/x` for complex expressions.
4. Test regex behavior with representative inputs.
5. Avoid regex when a parser or structured format module is more correct.

```perl
my $pattern = qr/
    \A
    (?<name>[a-z0-9_-]+)
    @
    (?<domain>[a-z0-9.-]+)
    \z
/ix;
```

## External Commands

1. Avoid shell interpolation with untrusted input.
2. Prefer list-form `system` when possible.
3. Check command exit status.
4. Capture and handle command output deliberately.

```perl
system 'git', 'status', '--short';
die "git status failed: $?" if $? != 0;
```

## Modules And Dependencies

1. Prefer core modules where they fit.
2. Use well-maintained CPAN modules for common formats and protocols.
3. Do not hand-roll JSON, CSV, URI, or date parsing.
4. Keep dependency choices conservative for operational scripts.
5. Isolate project-specific helpers into modules instead of copying functions
   across scripts.

## Error Handling

1. Fail loudly when the script cannot continue safely.
2. Use clear messages that include the failing operation.
3. Do not silently ignore bad input.
4. Convert external errors into useful messages for the caller.
5. Return meaningful exit codes from command-line scripts.

## Tooling

1. Format code with `perltidy` when available.
2. Use `perlcritic` for style and maintainability checks.
3. Run `perl -c` before committing script changes.
4. Add tests for modules with `Test::More`.

```bash
perl -c script.pl
prove -lr t
```

## Review Checklist

1. Are `strict` and `warnings` enabled?
2. Are file and command failures checked?
3. Is untrusted input validated?
4. Are regular expressions readable and tested?
5. Is shell interpolation avoided?
6. Is the script usable in automation?
