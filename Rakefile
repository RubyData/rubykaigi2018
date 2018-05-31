@docker_image_name = 'rubydata/rubykaigi2018'

namespace :docker do
  desc "Build docker image"
  task :build do
    Dir.chdir File.expand_path('../docker', __FILE__) do
      no_cache = ['--no-cache'] if ENV['no_cache']
      sh 'docker', 'build', *no_cache, '-t', "#{@docker_image_name}:latest", '.'
    end
  end

  desc "Push docker image"
  task :push do
    Dir.chdir File.expand_path('../docker', __FILE__) do
      sh 'docker', 'push', "#{@docker_image_name}:latest"
    end
  end

  desc "Pull docker image"
  task :pull do
    sh 'docker', 'pull', @docker_image_name
  end

  namespace :run do
    def normalize_volume(volume)
      local, remote = volume.split(':', 2)
      "#{File.expand_path(local)}:#{remote}"
    end

    def docker_run(*args)
      port = ['-p', "#{ENV['port']}:#{ENV['port']}"] if ENV['port']

      volume = ['-v', normalize_volume(ENV['volume'])] if ENV['volume']

      system 'docker', 'run', '--rm', '-it', *port, *volume,
             @docker_image_name, *args
    end

    def docker_run_jupyter(app)
      ENV['port'] ||= '8888'
      case app
      when :notebook
        ENV['ENABLE_JUPYTER_LAB'] = nil
      when :lab
        ENV['ENABLE_JUPYTER_LAB'] = '1'
      end
      docker_run("start-notebook.sh",
                 "--port=#{ENV['port']}",
                 "--NotebookApp.token=rubykaigi2018"
                )
    end

    desc "Run jupyter notebook on docker"
    task :notebook do
      docker_run_jupyter :notebook
    end

    desc "Run jupyter notebook on docker"
    task :lab do
      ENV['JUPYTER_ENABLE_LAB'] = '1'
      docker_run_jupyter :lab
    end

    desc "Run jupyter notebook on docker"
    task :bash do
      docker_run("start.sh", "/bin/bash")
    end
  end
end
