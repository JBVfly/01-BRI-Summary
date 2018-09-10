

### Sample PHP of e-mail status

```PHP
<?php
session_start() ; 
...
?>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<html>
<head>
  <title>BRI Email Status</title>
  <link rel="stylesheet" type="text/css" href="brimaster5.css">
  <link rel="stylesheet" type="text/css" href="bristat_v.css">
</head>
<body>
<?php
//$inc_path = str_replace('public', 'include/', $DOCUMENT_ROOT);
check_uri();

$reqmeth = $_SERVER["REQUEST_METHOD"] ;  // Find out how we got to this page.

echo '<table class="wrappr">' ;
echo '<tr>'    ;
echo '<td class="logo_q2"><img src="BRI-logo-90.jpg" width="90" height="88" border="0" alt=""></td>'.LF ;
echo '<td>' ;
$uacctno = ' '  ; 
if (isset($_SESSION['usrid']))
{
	$uacctno = $_SESSION['usrid'] ; 
	echo Top_Remarks() ; 
}
else
{
//	echo 'Invalid log in '.$fuser->status.' '.$uid.'<br>' ;
	echo 'Invalid log in ('.session_id().')'  ;
	echo '</td>'.LF ;
	echo '</tr>'    ;
	echo '</table>' ;
	exit ;
}
echo '</td></tr>'.LF ;

echo '<tr><td colspan="2">' ;  // This cell wraps div table

if ( $reqmeth == 'POST' )
{
	//echo 'This is a POST <br />' ; 
	// May 2007 - User must be deleting some sessions - - - - - - - -
	$irowcnt = $_POST["rowcnt"] ;
	//echo 'rowcnt:'.strval($irowcnt).'< <br>' ;
	$isub = 0 ;
	$iDelCnt = 0 ;
	$sQuery = ... ;
   	for($isub=1;$isub<=$irowcnt;$isub++)
	{
		$varcb = sprintf('CB%05d', $isub) ;
		$cbox  = $_POST[$varcb] ;
		if ($cbox == 'Y')
		{
			$varsn = sprintf('SN%05d', $isub) ;
			$snum = $_POST[$varsn] ;
			if ($iDelCnt == 0)
				$sQuery .= sprintf(" fsession = '%s'", $snum) ;
			else
				$sQuery .= sprintf(" OR fsession = '%s'", $snum) ;
			$iDelCnt++ ;
		}  // if ($cbox == 'Y')
	} // for($isub=1;$isub<=$irowcnt;$isub++)
		
	if ($iDelCnt>0) 
	{
		$oConn = db_connect_i('U') ;
		//echo sprintf("query:%s::  <br />", $sQuery) ; 
		$result = $oConn->query($sQuery) ;
		$oConn->close() ; 
	}
	//else
	//	echo 'Nothing to delete <br />' ; 
	echo '<br />' ; 
	
} //if ( $reqmeth == 'POST' )

Sess_View($uacctno) ; 

echo '</td>'.LF ;
echo '</tr>'    ;
echo '</table>' ;


// = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =
function Sess_View($inAcct)
{
	$sHidden = "" ; 
	echo '<form name="bri_stat" method="post" action="bristat_v.php">' ; 

	$oConn = db_connect_i('T') ;
	//echo sprintf('Conn no:%s <br>',  $oConn->connect_errno) ;
	//echo sprintf('Conn err:%s <br>', $oConn->connect_error) ;

	$sQuery = ... ;
	//echo sprintf('query:%s< <br />', $sQuery) ;
	$result = $oConn->query($sQuery) ;
	$iRowCnt = $result->num_rows ;
	echo sprintf('Sessions:%d <br />', $iRowCnt) ;
	//echo sprintf('Conn no:%s <br />',  $oConn->connect_errno) ;
	
	echo '<div class="itm_table">' ; 
	echo '<div class="itm_row_head">'  ; 
	echo '<div class="itm_cell_head">&nbsp;</div>' ;
	echo '<div class="itm_cell_head">Name</div>' ;
	echo '<div class="itm_cell_head">Date</div>' ;
	echo '<div class="itm_cell_head"><span title="(S)ocial or (W)ork">Ver</span></div>' ;
	echo '<div class="itm_cell_head"><span title="Times link was viewed">Vw</span></div>' ;
	echo '<div class="itm_cell_head">Status</div>' ;
	echo '<div class="itm_cell_head">Sent to...</div>' ;
	echo '<div class="itm_cell_head">Remarks</div>' ;
	echo '<div class="itm_cell_head">Session Code</div>' ;
	echo '<div class="itm_cell_head"><span title="Test-taker can view the PDF report upon completion">Prt</span></div>' ;
	echo '<div class="itm_cell_head">Link</div>' ;
	echo '</div>' ;  // Close row
	$iSessRow = 0 ; 
	while ( $row = $result->fetch_assoc() )  // <-- Loop through all sessions 
	{
		$iSessRow++ ; 
		$s_sessno = $row['fsession'] ;
		$s_URL = sprintf('http://www.BaughRelationshipIndex.com/sa-form-5.php?sessid=%s', $s_sessno) ; 

		$sDivCell = 'itm_cell'     ; 
		$sDivCtr  = 'itm_cell_ctr' ; 
		if ( $iSessRow%3 == 0 && $iSessRow != $iRowCnt)
		{
			$sDivCell = 'itm_cell_ln'     ; 
			$sDivCtr  = 'itm_cell_ctr_ln' ; 
		}
	
		// See if this session is still good - - - - - - - - - -
		$bGoodSess = false ; 
		$sName = sprintf('%s, %s', $row['fnlast'], $row['fnfirst']) ; 
		if ($row['fStype']!='M')
		{
			//$sName = substr(trim($row['fnlast']).', '.trim($row['fnfirst']),0,18) ; 
			if ($row['fdone'] != 'Y') 
				$bGoodSess = true        ;
		}
		else
		{
			// This is a multi-use or repeat session - - - - - - -
			$sName = '(Rem:' . strval($row['fSrem']) . ', exp:' . $row['fSdate'] . ')' ; 
			$YMD   = $row['fSdate'] ; 
			$dDate = mktime(23,59,59, 
				intval(substr($YMD,5,2)), intval(substr($YMD,8,2)), intval(substr($YMD,0,4)) ) ; 
			if ($row['fSrem'] > 0 && $dDate > time() )
				$bGoodSess = true        ;
		}  // if ($query_data['fStype']!='M')	
	
	
		//if ( $row['fdone'] != 'Y' )
		if ( $bGoodSess )
			echo '<div class="itm_row">' .LF ; 
		else
			echo '<div class="itm_row_inact">' .LF ; 
	
		$sCB = sprintf('CB%05d', $iSessRow) ; 
		echo sprintf('<div class="%s"><input type="checkbox" name="%s" value="Y"/></div>', $sDivCell, $sCB) .LF ;	// <-- Col 1 - Check Box  	
		echo sprintf('<div class="%s">%s </div>', $sDivCell, $sName )          .LF ;	// <-- Col 2 - Name
		echo sprintf('<div class="%s">%s</div>',  $sDivCell, $row['fdate'])    .LF ;	// <-- Col 3 - Date
		echo sprintf('<div class="%s">%s</div>',  $sDivCtr,  $row['fversion']) .LF ;	// <-- Col 4 - Version
		if ($row['fViewCnt']>0)
			echo sprintf('<div class="%s">%d</div>',  $sDivCtr, $row['fViewCnt'] ) .LF;
		else
			echo sprintf('<div class="%s">&nbsp;</div>', $sDivCtr) .LF ;

		if (!$bGoodSess)
			echo sprintf('<div class="%s">Done</div>', $sDivCtr) .LF ;
		else
		{
			$s_href  = sprintf('<A href="mailto:%s?subject=BRI link', $row['femail']) ;
			$s_href .= sprintf('&body=%s">Send link</a>', $s_URL) ;
			echo sprintf('<div class="%s">%s</div>', $sDivCtr, $s_href) .LF ;
		}

		$sMail = $row['femail'] ; 
		if ($row['fsent'] != 'T')
			$sMail .= sprintf('<span title="%s">&nbsp;(**FAILED**)</span>', $row['fErrDesc']) ; 
		echo sprintf('<div class="%s">%s</div>', $sDivCell, $sMail )   .LF ;

		echo sprintf('<div class="%s">%s</div>', $sDivCell, $row['fremarks'] ) .LF ;
		echo sprintf('<div class="%s">%s</div>', $sDivCell, $s_sessno )        .LF ;
		if ($row['fprint']=='Y')
			echo sprintf('<div class="%s">Y</div>', $sDivCtr) .LF ;
		else
			echo sprintf('<div class="%s">&nbsp;</div>', $sDivCtr) .LF ;
		echo sprintf('<div class="%s">%s</div>', $sDivCell, $s_URL ) ;
		echo '</div>' .LF ;  // Close row
	
		$sHidden .= sprintf('<input type="hidden" name="SN%05d" value="%s" />', $iSessRow, $s_sessno) .LF  ; 
	}  // while ( $row = $result->fetch_assoc() ) 
	echo '</div> ' .LF ; // Close table
	$oConn->close() ; 
	echo '<br>&nbsp;&nbsp;&nbsp;<input value="Delete Selected Sessions" type="submit">'.LF ;
	echo $sHidden ;
	echo sprintf('<input type="hidden" name="rowcnt" value="%d" />', $iRowCnt ) .LF ; 
	echo '</form>' ; 
}  // function Sess_View()


// = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =
function Top_Remarks()
{
	$sRet = '<p class="stat">List of BRI sessions set up with the email option</p>' ; 
	
	$aRems = ['If the original email was not received there are several backups.' , 
			  'When you click on &quot;Send link&quot;, your e-mail program should open and an e-mail composed to recipient.',  
			  'To the far right is the link for each BRI session. You can copy-and-paste a link into an e-mail.', 
			  'As a last resort the session code can be entered directly in the BRI home page.' , 
			  'To clean out old sessions, you can click checkboxes, then click the delete button at the bottom.' ,
			  'When you see &quot;**FAILED**&quot; it means the original email failed to send for technical reasons. Hover the mouse over FAILED to see the reason.' ]  ;

	$sRet .= '<ul>'  ;
	foreach($aRems as $sRem)
			$sRet.= sprintf('<li>%s</li>', $sRem) ;  
	$sRet .= '</ul>' ; 
			  
	return  $sRet ;
}





?>

```
