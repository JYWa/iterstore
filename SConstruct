# Copyright (C) 2013 by Carnegie Mellon University.

import os

env = DefaultEnvironment()
env['row_type'] = ARGUMENTS.get('row', 'vector')
env['oplog_type'] = ARGUMENTS.get('oplog', 'vector')
env['build_debug'] = ARGUMENTS.get('debug', '0')

build_variants = COMMAND_LINE_TARGETS
if len(build_variants) == 0:
    build_variants = ['build']

if 'lint' in build_variants:
   SConscript('SConscript', variant_dir='lint', duplicate=1)

# Make a phony target to time stamp the last build success
def PhonyTarget(target, source, action):
    env.Append(BUILDERS = { 'phony' : Builder(action = action) })
    AlwaysBuild(env.phony(target = target, source = source))

def write_build_info(target, source, env):
    build_options = 'scons build=%s row=%s oplog=%s' % \
        (env['build_debug'], env['row_type'], env['oplog_type'])
    os.system('echo %s > build/last-build-info' % build_options)
    os.system('date >> build/last-build-info')

if 'build' in build_variants:
   builds = SConscript('SConscript', variant_dir='build', duplicate=1)
   PhonyTarget('build/write-build-info', builds, write_build_info)
