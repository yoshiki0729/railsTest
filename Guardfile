# A sample Guardfile
# More info at https://github.com/guard/guard#readme

## Uncomment and set this to only include directories you want to watch
# directories %w(app lib config test spec features) \
#  .select{|d| Dir.exist?(d) ? d : UI.warning("Directory #{d} does not exist")}

## Note: if you are using the `directories` clause above and you are not
## watching the project directory ('.'), then you will want to move
## the Guardfile to a watched dir and symlink it back, e.g.
#
#  $ mkdir config
#  $ mv Guardfile config/
#  $ ln -s config/Guardfile .
#
# and, you'll have to watch "config/Guardfile" instead of "Guardfile"

# guard :minitest do
  # with Minitest::Unit
  # watch(%r{^test/(.*)\/?test_(.*)\.rb$})
  # watch(%r{^lib/(.*/)?([^/]+)\.rb$})     { |m| "test/#{m[1]}test_#{m[2]}.rb" }
  # watch(%r{^test/test_helper\.rb$})      { 'test' }

  # with Minitest::Spec
  # watch(%r{^spec/(.*)_spec\.rb$})
  # watch(%r{^lib/(.+)\.rb$})         { |m| "spec/#{m[1]}_spec.rb" }
  # watch(%r{^spec/spec_helper\.rb$}) { 'spec' }

  # Rails 4
  # watch(%r{^app/(.+)\.rb$})                               { |m| "test/#{m[1]}_test.rb" }
  # watch(%r{^app/controllers/application_controller\.rb$}) { 'test/controllers' }
  # watch(%r{^app/controllers/(.+)_controller\.rb$})        { |m| "test/integration/#{m[1]}_test.rb" }
  # watch(%r{^app/views/(.+)_mailer/.+})                    { |m| "test/mailers/#{m[1]}_mailer_test.rb" }
  # watch(%r{^lib/(.+)\.rb$})                               { |m| "test/lib/#{m[1]}_test.rb" }
  # watch(%r{^test/.+_test\.rb$})
  # watch(%r{^test/test_helper\.rb$}) { 'test' }

  # Rails < 4
  # watch(%r{^app/controllers/(.*)\.rb$}) { |m| "test/functional/#{m[1]}_test.rb" }
  # watch(%r{^app/helpers/(.*)\.rb$})     { |m| "test/helpers/#{m[1]}_test.rb" }
  # watch(%r{^app/models/(.*)\.rb$})      { |m| "test/unit/#{m[1]}_test.rb" }
# end

  require "active_support/inflector"
  # Guardのマッチング規則を定義
  guard :minitest, all_on_start: false do
    watch(%r{^test/(.*)/?(.*)_test\.rb$})
    watch('test/test_helper.rb') { 'test' }
    watch('config/routes.rb') { interface_tests }
    watch(%r{app/views/layouts/*}) { interface_tests }
    watch(%r{^app/models/(.*?)\.rb$}) do |matches|
      ["test/models/#{matches[1]}_test.rb",
       "test/integration/microposts_interface_test.rb"]
    end
    watch(%r{^test/fixtures/(.*?)\.yml$}) do |matches|
      "test/models/#{matches[1].singularize}_test.rb"
    end
    watch(%r{^app/mailers/(.*?)\.rb$}) do |matches|
      "test/mailers/#{matches[1]}_test.rb"
    end
    watch(%r{^app/views/(.*)_mailer/.*$}) do |matches|
      "test/mailers/#{matches[1]}_mailer_test.rb"
    end
    watch(%r{^app/controllers/(.*?)_controller\.rb$}) do |matches|
      resource_tests(matches[1])
    end
    watch(%r{^app/views/([^/]*?)/.*\.html\.erb$}) do |matches|
      ["test/controllers/#{matches[1]}_controller_test.rb"] +
      integration_tests(matches[1])
    end
    watch(%r{^app/helpers/(.*?)_helper\.rb$}) do |matches|
      integration_tests(matches[1])
    end
    watch('app/views/layouts/application.html.erb') do
      'test/integration/site_layout_test.rb'
    end
    watch('app/helpers/sessions_helper.rb') do
      integration_tests << 'test/helpers/sessions_helper_test.rb'
    end
    watch('app/controllers/sessions_controller.rb') do
      ['test/controllers/sessions_controller_test.rb',
       'test/integration/users_login_test.rb']
    end
    watch('app/controllers/account_activations_controller.rb') do
      'test/integration/users_signup_test.rb'
    end
    watch(%r{app/views/users/*}) do
      resource_tests('users') +
      ['test/integration/microposts_interface_test.rb']
    end
    watch('app/controllers/relationships_controller.rb') do
      ['test/controllers/relationships_controller_test.rb',
       'test/integration/following_test.rb']
    end
  end

  # 指定のリソースに対応する統合テストを返す
  def integration_tests(resource = :all)
    if resource == :all
      Dir["test/integration/*"]
    else
      Dir["test/integration/#{resource}_*.rb"]
    end
  end

  # インターフェースが該当するすべてのテストを返す
  def interface_tests
    integration_tests << "test/controllers"
  end

  # 指定のリソースに対応するコントローラのテストを返す
  def controller_test(resource)
    "test/controllers/#{resource}_controller_test.rb"
  end

  # 指定のリソースに対応するすべてのテストを返す
  def resource_tests(resource)
    integration_tests(resource) << controller_test(resource)
  end

