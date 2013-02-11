# Puppet module to install composer

## Description

install php composer - from http://getcomposer.org/ with puppet-composer module

## Installation

    puppet module install --target-dir=/your/path/to/modules tPl0ch-composer

## Usage


#### Configuring the composer install

In your manifest.pp:

    # configure composer install - not nessecary, comes with sane defaults
    class { 'composer':
        target_dir      => '/usr/local/bin',
        composer_file   => 'composer', # could also be 'composer.phar'
        download_method => 'curl', # or 'wget'
    }

#### Creating a project

In your manifest.pp:

    # create a project
    composer::project { 'silex':
        project_name   => 'fabpot/silex-skeleton',  # REQUIRED
        target_dir     => '/vagrant/silex', # REQUIRED
        prefer_source  => true,
        stability      => 'dev', # Minimum stability setting
        keep_vcs       => false, # Keep the VCS information
        dev            => true, # Install dev dependencies
        repository_url => 'http://repo.example.com' # Custom repository URL
    }

#### Updating a project / Updating package(s)

In your manifest.pp:

    # update a project
    composer::exec { 'silex-update':
        cmd                  => 'update',  # REQUIRED
        cwd                  => '/vagrant/silex', # REQUIRED
        packages             => ['silex/silex', 'symfony/browser-kit'], # leave empty or omit to update whole project
        prefer_source        => false,
        prefer_dist          => false,
        dry_run              => false, # Minimum stability setting
        no_custom_installers => false, # No custom installers
        no_scripts           => false, # No script execution
        no_interaction       => true, # No interactive questions
        optimize             => false, # Optimize autoloader
        dev                  => false, # Install dev dependencies
    }

#### Installing a project

In your manifest.pp:

    # install a project - packages variable is ignored with 'install' cmd
    composer::exec { 'silex-install':
        cmd                  => 'install',  # REQUIRED
        cwd                  => '/vagrant/silex', # REQUIRED
        prefer_source        => false,
        prefer_dist          => false,
        dry_run              => false, # Minimum stability setting
        no_custom_installers => false, # No custom installers
        no_scripts           => false, # No script execution
        no_interaction       => true, # No interactive questions
        optimize             => false, # Optimize autoloader
        dev                  => false, # Install dev dependencies
    }

## TODO

- Add type 'composer::require'
- Add rspec test cases