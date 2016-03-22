require "bundler/gem_tasks"
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

Rake::Task['release'].enhance [:guard_on_master_branch, :edit_changelog]

