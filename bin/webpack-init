#!/usr/bin/env node

const download = require('download-git-repo');
const ora = require('ora');
const path = require('path');
const chalk = require('chalk');
const program = require('commander');
const inquirer = require('inquirer');
const exists = require('fs').existsSync;
const logger = require('../lib/logger');

const template = "gweltaz-calori/webpack-boilerplate";

/**
 * Usage.
 */
program.usage('[project-name]');

/**
 * Help.
 */
program.on('--help', () => {
    console.log('  Usage:');
    console.log();
    console.log(chalk.gray('    # create a new project with custom name'));
    console.log('    $ webpack init my-project');
    console.log();
});

/**
 * Settings.
 */
let rawName = "webpack-boilerplate";

program.parse(process.argv);

if (program.args.length > 0) {
    rawName = program.args[0];
}
const inPlace = !rawName || rawName === '.';
const name = inPlace ? path.relative('../', process.cwd()) : rawName;
const to = path.resolve(rawName || '.');

/**
 * Padding.
 */
console.log();
process.on('exit', () => {
    console.log();
});

/**
 * Check if the destination folder already exists
 */
if (exists(to)) {
    inquirer.prompt([{
        type: 'confirm',
        message: inPlace ? 'Generate project in current directory ?' : 'Target directory exists. Continue ?',
        name: 'ok'
    }]).then(answers => {
        if (answers.ok) {
            downloadBoilerplate();
        }
    }).catch(logger.fatal);
} else {
    downloadBoilerplate();
}


/**
 * Download git repo into destination folder
 */
function downloadBoilerplate() {
    const spinner = ora('downloading boilerplate');
    spinner.start();

    download(template, to, {}, err => {
        spinner.stop();

        if (err) {
            logger.fatal('Failed to download repo ' + template + ': ' + err.message.trim());
        }
        logger.success('Generated "%s".', name)
    });
}
