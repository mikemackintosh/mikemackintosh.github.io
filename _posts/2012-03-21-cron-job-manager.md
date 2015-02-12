---
layout: post
permalink: /cron-job-manager
title: "Cron Job Manager"
category: ["uncategorized"]
tags: cron cron-tab cronjob linux-timer php-2 php4 php5 random-task schedule schedule-task zepnik
---
# Cron Jobs
Any sysadmin with a hope to grow their future has come across cron jobs. They are at the fundamental core of a working system. 
# The Code
I still need to add doc blocks and examples, but here's code to wet your lips. [php]<?php class CronManager{
	/* replace with the cron prefix you wish to run */
	var $prefix_path = 'php -f file.php';

	public function __construct($cron_file){
		$this->cron\_file = $cron\_file; $this->header[] = "######################################################"; $this->header[] = "#"; $this->header[] = "# Automatic Crontab Manager"; $this->header[] = "# Created Automagically On: ".date('F j, Y, g:i a'); $this->header[] = "# Filename: ".$this->cron\_file; $this->header[] = "# Author - Mike Mackintosh - www.highonphp.com"; $this->header[] = "#"; $this->header[] = "######################################################"; if(file\_exists('/etc/cron.d/'.$this->cron\_file)){ exec('sudo chmod 777 /etc/cron.d/'.$this->cron\_file, $output); } else{ exec('sudo touch /etc/cron.d/'.$this->cron\_file .' && sudo chmod 777 /etc/cron.d/'.$this->cron\_file, $output); } } public function add\_time($minute, $hour, $day, $month, $week, $app\_path = NULL){ $this->line[] = "$minute $hour $day $month $week root {$this->prefix\_path} $app\_path"; } public function clean(){ file\_put\_contents('/etc/cron.d/'.$this->cron\_file, ''); } public function save(){ file\_put\_contents('/etc/cron.d/'.$this->cron\_file, implode("\n", $this->header)."\n\n" . implode("\n", $this->line)."\n\n"); } public function \_\_destruct(){ exec('sudo chmod 644 /etc/cron.d/'.$this->cron\_file); exec('sudo touch /etc/cron.d'); } public function decode(){ $content = file\_get\_contents('/etc/cron.d/'.$this->cron\_file); $lines = explode("\n", $content); $i = 1; foreach($lines as $line){ if(substr($line, 0, 1) != '#' && $line != ''){ $part = explode("\t", $line); $output[$i] = array('Minute' => $part[0], 'Hour' => $part[1], 'Day' => $part[2], 'Month' => $part[3], 'Week' => $part[4], 'User' => $part[5], 'File' => str\_replace($this->prefix\_path, '', $part[6])); $i++; } } return $output; } } [/php]