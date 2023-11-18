# Copyright (c) 2022-2023, PostgreSQL Global Development Group

pg_cron_sources = files(
  'src/entry.c',
  'src/job_metadata.c',
  'src/misc.c',
  'src/pg_cron.c',
  'src/task_states.c'
)

if host_system == 'windows'
  pg_cron_sources += rc_lib_gen.process(win32ver_rc, extra_args: [
    '--NAME', 'pg_cron',
    '--FILEDESC', 'pg_cron - simple cron-based job scheduler',])
endif

pg_cron = shared_module('pg_cron',
  pg_cron_sources,
  include_directories: include_directories('./', '../', './include/'),
  kwargs: contrib_mod_args + {
    'dependencies': contrib_mod_args['dependencies'] + [libpq],
  },
)
contrib_targets += pg_cron

install_data(
  'pg_cron.control',
  'pg_cron.sql',
  kwargs: contrib_data_args,
)

tests += {
  'name': 'pg_cron',
  'sd': meson.current_source_dir(),
  'bd': meson.current_build_dir(),
  'regress': {
    'sql': [
      'pg_cron',
    ],
    'regress_args': ['--dlpath', meson.build_root() / 'src/test/regress'],
  },
}
