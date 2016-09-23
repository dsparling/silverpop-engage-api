# silverpop-engage-api
Engage.php from Silverpop XML API Developer Guide

## Examples

## Example from Developer Guide
	<?php
	require_once __DIR__ . '/lib/Engage.php';
	/***********************************************************************/
	// Configuration section - replace with your values before running
	// replace with your Engage API endpoint hostname:
	$apiHost = 'api1.silverpop.com';
	// replace with your Engage API username:
	$username = 'user@host.com';
	// replace with your Engage API password:
	$password = 'xxxyz123';
	/***********************************************************************/
	try {
		echo "Logging into Engage API on {$apiHost} as {$username}\n";
	
		$engage = new Engage($apiHost);
		$engage->login($username, $password);
	
		echo "Fetching list of shared databases, queries, and contact lists\n";
	
		$request = '<GetLists><VISIBILITY>1</VISIBILITY><LIST_TYPE>2</LIST_TYPE></GetLists>';
		$response = $engage->execute($request);
	
		foreach ($response->RESULT->LIST as $list) {
	
			if ($list->TYPE == 0) { $type = 'Database';
			} elseif ($list->TYPE == 1) {
				$type = 'Query';
			} elseif ($list->TYPE == 18) {
				$type = 'Contact list';
			} else {
				$type = 'Unknown list type';
			}
	
			$id = $list->ID;
			$name = $list->NAME;
			$size = $list->SIZE;
	
			echo "{$type} ID {$list->ID} '{$list->NAME}' has {$list->SIZE} recipients\n";
		}
	
	} catch (Exception $e) {
		echo $e->getMessage() . "\n"; print_r($engage->getLastRequest());
		print_r($engage->getLastResponse());
		print_r($engage->getLastFault());
	}
	
