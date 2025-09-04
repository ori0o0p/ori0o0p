#### Personal Domain

- portfolio : [**seungwon.me**](https://seungwon.me)
  
- blog      : [**seungwon.tech**](https://seungwon.tech)

#### Open Source Contributions

- 2025.08.08 **[redis/lettuce](https://github.com/redis/lettuce) -** [#3266](https://github.com/redis/lettuce/pull/3266)

- 2025.05.28 **[redis/lettuce](https://github.com/redis/lettuce) -** [#3264](https://github.com/redis/lettuce/pull/3264)

- 2025.04.23 **[redis/lettuce](https://github.com/redis/lettuce) -** [#3262](https://github.com/redis/lettuce/pull/3262)
  
- 2025.01.06 **[redis/lettuce](https://github.com/redis/lettuce) -** [#3061](https://github.com/redis/lettuce/pull/3061)

- 2024.11.25 **[kestra-io/kestra](https://github.com/kestra-io/kestra) -** [#6073](https://github.com/kestra-io/kestra/pull/6073)

- 2024.10.08 **[velog-io/velog](https://github.com/velog-io/velog) -** [#49](https://github.com/velog-io/velog/pull/49)



```
reset() {
  clear
  exec $SHELL -l
}
    
delete() {
  local target="$1"
  if [[ -z "$target" ]]; then
    echo "사용법: delete <파일명|디렉토리명>"
    return 1 
  fi
  
  if [[ -f "$target" ]]; then
    rm "$target"
    echo "파일이 삭제되었습니다: $target"
  elif [[ -d "$target" ]]; then
    rm -r "$target"
    echo "디렉토리가 삭제되었습니다: $target"
  else
    echo "해당 파일 또는 디렉토리가 존재하지 않습니다: $target"
    return 1
  fi
} 

  
memo() {
  # 현재 시간: YYYY-MM-DD_HH-MM-SS 형식
  local filename
  filename="$(date '+%Y-%m-%d_%H-%M-%S').txt"
  touch "$filename"
  echo "메모 파일이 생성되었습니다: $filename"
  nano "$filename"
}
  
on() {
  local file="$1"
  if [[ -z "$file" ]]; then
    echo "사용법: on <파일명>"
    return 1
  fi
  if [[ -f "$file" ]]; then
    nano "./$file"
  else
    echo "해당 파일이 존재하지 않습니다."
    return 1
  fi
}


# 디렉토리 생성 함수
make_dir() {
  local dir="$1"
  if [[ -z "$dir" ]]; then
    echo "사용법: make_dir <디렉토리명>"
    return 1
  fi
  if [[ -d "$dir" ]]; then
    echo "이미 디렉토리가 존재합니다: $dir"
  else
    mkdir -p "$dir"   
    echo "디렉토리 생성 완료: $dir"
  fi
}

current_branch() { 
  git rev-parse --abbrev-ref HEAD
}
    
gitlog() {  
  local count=${1:-20}
  git log --oneline --graph --decorate -n "$count"
}
  
changed_files() {
  git status --short
}
  
diff_all() {
  echo "Unstaged changes:"
  git diff
  echo "Staged changes:"
  git diff --cached
}
  
gcm() {
  git add .
  git commit -m "$*"
}   
  
undo_commit() {   
  git reset --soft HEAD~1
  echo "최근 커밋을 되돌렸습니다. 변경사항은 워킹 디렉토리에 남아있습니다."
}
    
rollback_soft() {
  echo "소프트 롤백을 진행합니다. 복구가 가능합니다."
  echo -n "롤백을 하시겠습니까? (y/n): "
  read ans
  if [[ "$ans" == "y" || "$ans" == "Y" ]]; then
    echo "변경 파일 목록:"
    git status --short
    echo "diff:"
    git diff
    # 변경사항을 stash에 저장 (untracked 포함)
    git stash push -u -m "rollback_soft backup $(date '+%Y-%m-%d %H:%M:%S')"
    git restore .
    git clean -fd
    echo "소프트 롤백 완료! rollback_undo로 복구할 수 있습니다."
  else
    echo "롤백을 진행하지 않습니다."
  fi
}

rollback_undo() {
  echo "롤백을 복구합니다!"
  git stash list | grep 'rollback_soft backup' > /dev/null
  if [[ $? -eq 0 ]]; then
    git stash pop
    echo "롤백 이전 상태로 복구되었습니다."
  else
    echo "복구할 stash가 없습니다."
  fi  
}
    
rollback_hard() {
  echo "하드 롤백을 진행합니다. 복구가 불가능합니다."
  echo -n "정말 롤백을 하시겠습니까? (y/n): "
  read ans
  if [[ "$ans" == "y" || "$ans" == "Y" ]]; then
    echo "변경 파일 목록:"
    git status --short
    echo "diff:"
    git diff
    # 변경사항 완전 삭제 (백업 없음)
    git restore .
    git clean -fd
    echo "하드 롤백 완료! 복구가 불가능합니다."
  else
    echo "롤백을 진행하지 않습니다."
  fi
}
  
  
myip() {
  curl -s ifconfig.me
}
  
cdd() {
  cd "$1" && ls
}
    
backup() {
  cp "$1" "$1.$(date +%Y%m%d_%H%M%S).bak"
}

```
