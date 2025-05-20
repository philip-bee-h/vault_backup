---
type: reference
refrenceName: bash-prompt-config
tags:
  - bash
  - terminal
source: "{{source}}"
date: 2024-05-08
---

### rwxrob prompt

**Reference dotfiles:** https://github.com/rwxrob/dot/blob/main/.bashrc

```bash
# --------------------------- smart prompt ---------------------------
#                 (keeping in bashrc for portability)

PROMPT_LONG=20
PROMPT_MAX=95
PROMPT_AT=@

__ps1() {
	local P='$' dir="${PWD##*/}" B countme short long double \
		r='\[\e[31m\]' g='\[\e[30m\]' h='\[\e[34m\]' \
		u='\[\e[33m\]' p='\[\e[34m\]' w='\[\e[35m\]' \
		b='\[\e[36m\]' x='\[\e[0m\]'

	[[ $EUID == 0 ]] && P='#' && u=$r && p=$u # root
	[[ $PWD = / ]] && dir=/
	[[ $PWD = "$HOME" ]] && dir='~'

	B=$(git branch --show-current 2>/dev/null)
	[[ $dir = "$B" ]] && B=.
	countme="$USER$PROMPT_AT$(hostname):$dir($B)\$ "

	[[ $B == master || $B == main ]] && b="$r"
	[[ -n "$B" ]] && B="$g($b$B$g)"

	short="$u\u$g$PROMPT_AT$h\h$g:$w$dir$B$p$P$x "
	long="$g╔ $u\u$g$PROMPT_AT$h\h$g:$w$dir$B\n$g╚ $p$P$x "
	double="$g╔ $u\u$g$PROMPT_AT$h\h$g:$w$dir\n$g║ $B\n$g╚ $p$P$x "

	if ((${#countme} > PROMPT_MAX)); then
		PS1="$double"
	elif ((${#countme} > PROMPT_LONG)); then
		PS1="$long"
	else
		PS1="$short"
	fi
}

PROMPT_COMMAND="__ps1"

```

