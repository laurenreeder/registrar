fs = require 'fs'
{spawn} = require 'child_process'
{print} = require 'sys'

# TODO: Determine a better convention

srcCoffeeDir         = '.'
targetJsDir          = '.'

testSrcCoffeeDir     = 'test/'
testTargetJsDir      = 'test/'


build = (cb, src, target) ->
  ps = spawn "coffee", ['-c', '-o', target, src]
  ps.stdout.on 'data', (data) ->
    print data.toString()
  ps.stderr.on 'data', (data) ->
    # ?
    process.stderr.write data.toString()

  ps.on 'exit', (code) ->
    if code != 0
      console.log 'failed'
    else
      cb?()

buildSrc = (cb) ->
  build cb, srcCoffeeDir, targetJsDir

buildTests = (cb) ->
  build cb, testSrcCoffeeDir, testTargetJsDir

test = (cb) ->
  buildSrc ->
    buildTests ->
      ps = spawn "mocha"
      ps.stdout.on 'data', (data) ->
        print data.toString()
      ps.stderr.on 'data', (data) ->
        # ?
        process.stderr.write data.toString()

      ps.on 'exit', (code) ->
        if code != 0
          console.log 'failed'
        else
          cb?()

task 'build', 'compile code', buildSrc
task 'test', 'run tests', test
