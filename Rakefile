require "bundler/gem_tasks"
Bundler::GemHelper.install_tasks
ENV['gem_push'] = 'off'

def with_fixed_editor
  editor = ENV['EDITOR'] || ""
  abort "You must set an EDITOR to edit the changelog" if editor.empty?
  swaps = {
    "mate" => "mate -w",
    "subl" => "subl -w",
    "mvim" => "mvim -f"
  }
  begin
    ENV['EDITOR'] = swaps.fetch(editor, editor)
    yield
  ensure
    ENV['EDITOR'] = editor
  end
end
task :guard_on_master_branch do
  unless `git branch` =~ /^\* master$/
    abort "You must be on the master branch to release."
  end
end
task :edit_changelog do
  unless `which git-changelog`.empty?
    with_fixed_editor {
      sh "git-changelog"
    }
  else
    abort "git-changelog isn't found. Install it with `brew install git-extras`"
  end
end

Rake::Task[:release].enhance [:guard_on_master_branch, :edit_changelog]

#with_fixed_editor
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

## Invoke the rake tasks in the proper order for our releases
#task :prepare_release do
  #Rake::Task['guard_on_master_branch'].invoke
  #Rake::Task['release:guard_clean'].invoke
  #Rake::Task['edit_changelog'].invoke
  #Rake::Task['build'].invoke
  #Rake::Task['release:source_control_push'].invoke
#end

## Override the `rake release` provided by Bundler so we can change the order
#task(:release).clear

#desc "Add to the changelog and release the gem"
#task :release => [:prepare_release]

## module Bundler
##   class GemHelper
##     # Override Bundler's concept of release.
##
##     def release_gem(built_gem_path=nil)
##       with_fixed_editor {
##         guard_on_master_branch
##         raise "This tag has already been committed to the repo" if already_tagged?
##         built_gem_path ||= build_gem
##         edit_changelog
##         sh "git commit --allow-empty -a -m 'Release #{version_tag}'"
##         tag_version {
##           # Bundler's git_push pushes all branches. Let's restrict it
##           # to only the master branch since we also ensure that you
##           # always release from the master branch.
##           perform_git_push "origin master --tags"
##         }
##       }
##     end
##     def with_fixed_editor
##       editor = ENV['EDITOR'] || ""
##       fail "You must set an EDITOR to edit the changelog" if editor.empty?
##       swaps = {
##         "mate" => "mate -w",
##         "subl" => "subl -w",
##         "mvim" => "mvim -f",
##         "vim"  => "vim -f"
##       }
##       begin
##         ENV['EDITOR'] = swaps.fetch(editor, editor)
##         yield
##       ensure
##         ENV['EDITOR'] = editor
##       end
##     end
##     def guard_on_master_branch
##       unless `git branch` =~ /^\* master$/
##         abort "You must be on the master branch to release."
##       end
##     end
##     def edit_changelog
##       unless `which git-changelog`.empty?
##         with_fixed_editor {
##           sh "git-changelog"
##         }
##       else
##         fail "git-changelog isn't found. Install it with `brew install git-extras`"
##       end
##     end
##   end
## end


## desc "Add to the changelog and release the gem"
## task :release, [:built_gem_path] do |t, args|
##   built_gem_path = args[:built_gem_path] || nil
##   Bundler::GemHelper.new.release_gem(built_gem_path)
## end

