PUPPETMASTER='kermit.vmware'
SSH='ssh -t -A'

task :deploy do
	sh "git push"
	sh "#{SSH} #{PUPPETMASTER} 'cd /etc/puppet && git pull'"
end

task :apply => [:deploy] do
	client = ENV['CLIENT']
	sh "#{SSH} #{client} 'puppet agent --test --server=#{PUPPETMASTER}'" do |ok,
	status|
		puts case status.exitstatus
			when 0 then "Client is up to date."
			when 1 then "Puppet couldn't compile the manifest."
			when 2 then "Puppet made changes."
			when 4 then "Puppet found errors."
		end
	end
end
