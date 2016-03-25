<?php

abstract class ILoger
{
	abstract public function log($message);
	
}

class StdOutLoger extends ILoger
{
	public function log($message)
	{
		echo $message, " ", date('Y-m-d H:i:s');
	}
	
}

class FileLoger extends ILoger
{
	var $filePath;	
	
	public function setFilePath($_filePath)
	{
		$this->filePath=$_filePath;
	}

	public function log($message)
	{
		if($fp=fopen($this->filePath,'a'))
		{
		fputs($fp,date('Y-m-d H:i:s')." ".$message."\n");
		fclose($fp);
		}
		else throw new Exception('File not found!');
		
	}
}

class DBLoger extends ILoger
{
	var $dbName;
	var $host;
	var $user;
	var $pass;

	public function __construct()
	{
		$this->dbName='myDB';
		$this->host='localhost';
		$this->user='root';
		$this->pass='12345';
	}

	public function setParam($_dbName,$_host,$_user,$_pass)
	{
		$this->dbName=$_dbName;
		$this->host=$_host;
		$this->user=$_user;
		$this->pass=$_pass;
	}

	public function log($message)
	{
		if ($l=mysql_connect($this->host,$this->user,$this->pass))
		{
			mysql_query("CREATE DATABASE IF NOT EXISTS $this->dbName ",$l);
			mysql_select_db($this->dbName,$l);
			mysql_query('SET NAMES UTF8',$l);
			mysql_query("CREATE TABLE IF NOT EXISTS logs
					(id INT NOT NULL AUTO_INCREMENT,
					message CHAR(200) NOT NULL,
					data CHAR(20) NOT NULL,
					PRIMARY KEY(id));",$l);
			$res=mysql_query("INSERT INTO logs (id,$message,data) VALUES (0,$message,date('Y-m-d H:i:s'));",$l);

			if ($res = 'true') echo "Information in DB";
			else echo "Information NOT in DB";

			if(!mysql_close($l)) throw new Exception('Connect not close');
		}
		else throw new Exception('Connect is loss');		
	}
}
?>
