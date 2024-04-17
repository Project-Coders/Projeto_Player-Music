# Função para obter a data atual no formato desejado

get_current_date() {
LC_TIME=en_US.UTF-8 date +'%A/%B/%Y'
}

# Função para obter a hora atual no formato desejado

get_current_time() {
date +'%I:%M%p'
}

# Função para retornar a branch como "main" ou "master"

get_current_branch() {
local branch_name
branch_name=$(git symbolic-ref --short HEAD 2>/dev/null)
  echo "$branch_name"
}

# Função para verificar o estado do repositório Git

git_prompt_info() {
local branch_name
branch_name=$(git rev-parse --abbrev-ref HEAD 2>/dev/null)

if [[$? -eq 0]]; then
local has_changes
has_changes=$(git status --porcelain)

    if [[ -n "$has_changes" ]]; then
      echo "🚀"
    else
      echo "⚡"
    fi

fi
}

# Função para verificar se está no diretório raiz do Git (main ou master)

is_git_root_directory() {
git rev-parse --is-inside-work-tree &>/dev/null
}

is_home_directory() {
[["$PWD" == "$HOME"]]
}

# Função para exibir o nome do diretório atual ou ícone do diretório raiz

current_directory() {
if is_home_directory; then
echo "📁"
else
echo "%c📂"
fi
}

# Configurações do prompt do Git e Mercurial

ZSH_THEME_GIT_PROMPT_PREFIX="λ %F{#c661f5}git:%F{#ff145e}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%F{#efff14}→ %{$reset_color%}"
ZSH_THEME_HG_PROMPT_PREFIX="λ %F{#c661f5}hg %F{#ff145e}"
ZSH_THEME_HG_PROMPT_SUFFIX="%F{#efff14} → %{$reset_color%}"

# Define o prompt principal com informações básicas

PROMPT='%F{#c661f5}%B✘ %F{#efff14}Mxt@DEV:%F{#14ff43}($(current_directory)%F{#14ff43})%F{#ff145e} ~$(if is_git_root_directory; then echo " %F{#70caf0}git:%F{#ff145e}$(get_current_branch)%F{#c661f5}[$(git_prompt_info)%F{#c661f5}]";fi) %F{#14ff43}▌%F{#c661f5}$(get_current_time) %F{#ff145e}♦ %F{#c661f5}$(get_current_date)%F{#14ff43}▐%{$reset_color%}%B'$'\n%F{#14ff43}↳%F{#fffffff} '
