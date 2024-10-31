# 🔍 qse

Useful recipes of various fzf+rga+bat integrations

## Summary
- [About](#about)
- [Recipes](#recipes)
- [Advanced](#advanced)
- [Installation](#installation)

## About

This will repo will help you to search for code faster than ever.

## Recipes

Basic version of qse:

```bash
qse() {
	RG_PREFIX="rg --files-with-matches"
	local file
	file="$(
		FZF_DEFAULT_COMMAND="$RG_PREFIX '$1'" \
			fzf \
				--preview="[[ ! -z {} ]] && rg --pretty --context 5 {q} {}" \
				--disabled --query "$1" \
				--bind "change:reload:sleep 0.1; $RG_PREFIX {q}" \
				--bind "f3:execute(bat --paging=always --pager=less --color=always {} < /dev/tty > /dev/tty)" \
				--bind "f4:execute(code {})" \
				--preview-window="70%:wrap"
	)" &&
	echo "$file"
}
```

## Advanced

Advanced version with `bat`-preview (and awesome [batgrep](https://github.com/eth-p/bat-extras/blob/master/doc/batgrep.md))

```bash
export BAT_THEME="Monokai Extended"
qse() {
	RG_PREFIX="rg --files-with-matches"
	local file
	file="$(
		FZF_DEFAULT_COMMAND="$RG_PREFIX '$1'" \
			fzf \
			--preview="if [[ -n {} ]]; then if [[ -n {q} ]]; then ~/bin/bat-extras/src/batgrep.sh --color=always --terminal-width=105 --context=3 {q} {}; else bat --color=always {}; fi; fi" \
			--disabled --query "$1" \
			--bind "change:reload:sleep 0.1; $RG_PREFIX {q}" \
			--bind "f3:execute(bat --paging=always --pager=less --color=always {} < /dev/tty > /dev/tty)" \
			--bind "f4:execute(code {})" \
			--preview-window="70%:wrap"
	)" &&
	echo "$file"
}
```

## Installation

Install [bat](https://github.com/sharkdp/bat?tab=readme-ov-file#installation), [fzf](https://github.com/junegunn/fzf?tab=readme-ov-file#installation), [ripgrep](https://github.com/BurntSushi/ripgrep?tab=readme-ov-file#installation)

Then simply add recipe to your ~/.shrc or ~/.zshrc and restart the shell
