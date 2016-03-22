require "bundler/gem_tasks"
ENV['gem_push'] = 'off'

#def with_fixed_editor
  #editor = ENV['EDITOR'] || ""
  #fail "You must set an EDITOR to edit the changelog" if editor.empty?
  #swaps = {
    #"mate" => "mate -w",
    #"subl" => "subl -w",
    #"mvim" => "mvim -f"
  #}
  #begin
    #ENV['EDITOR'] = swaps.fetch(editor, editor)
    #yield
  #ensure
    #ENV['EDITOR'] = editor
  #end
#end
#task :guard_on_master_branch do
  #unless `git branch` =~ /^\* master$/
    #fail "You must be on the master branch to release."
  #end
#end
#task :edit_changelog do
  #unless `which git-changelog`.empty?
    #with_fixed_editor {
      #sh "git-changelog"
    #}
  #else
    #fail "git-changelog isn't found. Install it with `brew install git-extras`"
  #end
#end


def release_gem(built_gem_path=nil)
  with_fixed_editor {
    guard_on_master_branch
    raise "This tag has already been committed to the repo" if already_tagged?
    built_gem_path ||= build_gem
    edit_changelog
    sh "git commit --allow-empty -a -m 'Release #{version_tag}'"
    tag_version {
      # Bundler's git_push pushes all branches. Let's restrict it
      # to only the master branch since we also ensure that you
      # always release from the master branch.
      perform_git_push "origin master --tags"
    }
  }
end
def with_fixed_editor
  editor = ENV['EDITOR'] || ""
  abort "You must set an EDITOR to edit the changelog" if editor.empty?
  swaps = {
    "mate" => "mate -w",
    "subl" => "subl -w"
  }
  begin
    ENV['EDITOR'] = swaps.fetch(editor, editor)
    yield
  ensure
    ENV['EDITOR'] = editor
  end
end
def guard_on_master_branch
  unless `git branch` =~ /^\* master$/
    abort "You must be on the master branch to release."
  end
end
def edit_changelog
  unless `which git-changelog`.empty?
    sh "git-changelog"
  else
    abort "git-changelog isn't found. Install it with `brew install git-extras`"
  end
end

task(:release).clear

desc "Add to the changelog and release the gem"
task :release, [:built_gem_path] do |t, args|
  built_gem_path = args[:built_gem_path] || nil
  release_gem(built_gem_path)
end

