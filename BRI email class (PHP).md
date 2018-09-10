
This PHP class inherits from PHPmailer and handles e-mail sent out by the BRI website.

```PHP

<?php 

includes ...

//require_once('sess_class.php') ;

class JVMailer extends PHPMailer
{

	private $m_Acct, $m_Action ;
	
	//private $m_ToAddr ; 
	private $m_NameFirst, $m_NameLast, $m_Email, $m_FullName ; 
	private $m_Version, $m_Random ; 
	private $m_Sess1, $m_Sess2 ; 
	private $m_Rt, $m_Bcc ; 
	private $m_Nc ;  // <-- 25 July '15 - Notify complete 
	private $m_Prt, $m_Rem, $m_Mat, $m_Omit ; 
	
	private $m_MsgSubject, $m_MsgBody ;  // <-- Added 13 June 2018
	private $m_sErrInfo ; 
	private $m_sDebug ; 
	private $m_RefName = ' ' ; 
	
	private $m_MailResult ; 
		
	public $m_ToAddr ; 	
		
	// Constructor = = = = = = = = = = = = = = = = =
	//function JVMailer($pAcct, $pAction)  
	function __construct($inAcct, $inAction)
	{
		$this->m_Acct   = $inAcct   ;
		$this->m_Action = $inAction ;
		
		// Initialize class variables - - - - - - - - -
		$this->m_NameFirst = 'First?' ;
		$this->m_NameLast  = 'Last?'  ;
		$this->m_ToAddr    = 'FlyJohnV@comcast.net' ;
		
		$this->m_MailResult = 0 ;  // <-- Later set to 1 for success or -1 for failer 
		
		// Add 13 June 2018 - - - -
		$this->m_Sess1 = "" ; 
		$this->m_Sess2 = "" ; 
		$this->m_MsgSubject = "?" ; 
		$this->m_MsgBody  = "?" ; 
		$this->m_sErrInfo = '' ; 
		$this->m_sDebug = sprintf("<br/>Construct Acct:%s, Act:%s", $inAcct, $inAction)  ; 

		// 14 June 2018 - Set a generic body - - - - - -
		$this->m_MsgSub = 'No subject set' ; 
		$sBody  = $this->BodyHeader() ; 
		$sBody .= sprintf('<div>No content set [%s]</div>', $inAction) ;  
		$sBody .= $this->BodyFooter() ; 
		$this->m_MsgBody = $sBody ; 
	}
	
	function SetNameEmail($inFirst, $inLast, $inEmail)
	{
		$this->m_NameFirst = $inFirst ;
		$this->m_NameLast  = $inLast  ;
		$this->m_ToAddr    = $inEmail ;
		
		$this->m_FullName = trim($this->m_NameFirst . ' ' . $this->m_NameLast) ; 
	}

	function SetRefName($inName)
	{
		$this->m_RefName = $inName ;
	}

	function SetVerRand($inVer, $inRand)
	{
		$this->m_Version = $inVer  ;
		$this->m_Random  = $inRand ;
	}

	function SetSess($inSess1, $inSess2)
	{
		$this->m_Sess1  = $inSess1 ;
		$this->m_Sess2  = $inSess2 ;
	}

	function SetRtBccNc($pRt, $pBcc, $pNc)
	{
		$this->m_Rt  = $pRt   ;
		$this->m_Bcc = $pBcc  ;
		$this->m_Nc  = $pNc   ;
	}

	function SetPrtRemMatOmit($pPrt, $pRem, $pMat, $pOmit)
	{
		$this->m_Prt  = $pPrt  ;
		$this->m_Rem  = $pRem  ;
		$this->m_Mat  = $pMat  ;
		$this->m_Omit = $pOmit ;
	}
	
	function SetSubBody($inSub, $inBody)
	{
		if ($inSub > ' ')	$this->m_MsgSubject = $inSub  ;
		if ($inBody > ' ')	$this->m_MsgBody    = $inBody ; 
		
	} // function SetSubBody($inSub, $inBody)

	function BodyHeader()
	{
		$sRet  = '<!DOCTYPE html>' ; 
		$sRet .= '<html><head><style type="text/css">'.LF  ;
		$sRet .= 'body { background-color #FEFEFE; font-family:Arial; color:#464646; }'.LF ;
		$sRet .= 'div.divcls { background-color: LightSalmon; }'.LF ;
		$sRet .= 'div.footr  { color: DarkSalmon;   margin-top: 120px; }'.LF ;
		$sRet .= '</style></head>'.LF ;
		$sRet .= '<body>'.LF ; 
		$sRet .= '<img src="http://www.baughrelationshipindex.com/BRI-logo-45.jpg" alt="BRI"  width="45" height="44">' ;  
		$sRet .= "<br><br>"  ;	
		
		return $sRet ; 
	}  // function SetBodyHeader()	


	function BodyFooter()
	{
		$sRet = '<br/>' ; 
		$sRet .= sprintf('<div class="footr">%s</div>', $this->m_Action) ;
		$sRet .= '</body> </html>' ;
		return $sRet ; 
	}	
	
	function GetMailResult()
	{
		
		return $this->m_MailResult ; 
	}

	function GetErrInfo()
	{
		return $this->m_sErrInfo ; 
	}
	
	
	function GetDebug()
	{
		return sprintf("{{%s}}   lengh:%d", $this->m_sDebug, strlen($this->m_sDebug) ) ; 
		
	}

	function SendNow()
	{
		$SrcIP = get_ip() ;
		$ufields = new user_fields_cls($this->m_Acct) ;  // <-- sess_class.php
	
		$sMsg = 'Not doing anything yet...  <br><br>' ; 

		$this->IsSMTP() ;  // Set mailer to use SMTP
		$this->Host     = ... ;  

		$this->Username = ...   // SMTP username
		$this->Password = ...   // SMTP password
		$this->SMTPAuth = true ;   // Enable SMTP authentication
		$this->Port     = 587  ;   // Set the SMTP  (Insecure Transport - No SSL function enabled) 
		//$oMail->Port  = 465  ;   // (Secure Transport   - SSL function enabled) 
		//$oMail->Port  = 25   ;   //  (username/password authentication MUST also be enabled!) 
		$this->SMTPSecure = 'tls' ;
		
		// 5 Oct '15 changed needed with PHP 5.6 
		//     ...and it looks like it is working
		$this->SMTPOptions = array(
			    'ssl' => array(
		        'verify_peer' => false,
		        'verify_peer_name' => false,
    		    'allow_self_signed' => true     )  ) ;

		//$bWriteSessFile = false ; 
		
		$bInsertSession = false ;
		if ($this->m_Action == '1/1' || $this->m_Action == '1/2' || $this->m_Action == '2/2')	
		{
			$this->m_MsgBody = $this->BuildBRImail($ufields->emladdr, $ufields->dispname) ; 
			$bInsertSession  = true ;
		}
		elseif (substr($this->m_Action,0,4) == 'Eval')
			{
				$this->m_MsgBody = $this->BuildSample() ;    // $body ; 
				$bInsertSession  = true ;
			}
			elseif ($this->m_Action == 'NotifyComp')
				{
					$this->m_MsgBody = $this->BuildNotify($ufields->emladdr, $ufields->dispname) ;    // $body ; 
				}
				elseif ($this->m_Action == 'X')
					{
						$this->AddAddress($this->m_ToAddr, $this->m_FullName ) ;
						$this->SetFrom('no-reply@baughrelationshipindex.com', 'Baugh Relationship Index') ;
						$body  = "<br>From JVMailer..." ;
						$body .= "<br><br><br><br><br><br>Experimental"  ;
						$this->m_MsgBody = $body ;    // $body ; 
					}
					else
					{
						$this->AddAddress($this->m_ToAddr, $this->m_FullName ) ;
						$this->SetFrom('no-reply@baughrelationshipindex.com', 'Baugh Relationship Index') ;
					}

		$this->Subject = $this->m_MsgSub  ; 
		$this->Body    = $this->m_MsgBody ;
		$this->isHTML(true) ;

		$this->m_sErrInfo = "" ; 
		$mailsent = 'F' ;
		if(!$this->Send()) 
		{
			// Email send failed - - - - - - - - - - - - - - - - - - - -
			$this->m_MailResult = -1 ; 
			$this->m_sErrInfo = $this->ErrorInfo ; 

			$sMsg = "Error sending e-mail to:" . $this->m_FullName . "  ( " . $this->m_ToAddr . " )" ; 
			$sMsg .= "<br>Error:" . $this->m_sErrInfo . " to: " ;

			$sTran = sprintf("Email failed Act:%s, Addr:%s, Name:%s, Error:%s ", 
							$this->m_Action,   $this->m_ToAddr, 
							$this->m_FullName, $this->m_sErrInfo)  ;
			LogTrans($this->m_Acct, $sTran) ;
			
		}
		else
		{
			// Email successful - - - - - - - - - - - - - - - - - - - - -
			$this->m_MailResult = 1 ; 
			$sMsg = "BRI message sent to " . $this->m_FullName . '  ( ' . $this->m_ToAddr . ' ) '  ;

			$sTran = sprintf("Email send  Act:%s, Name:%s (%s)", $this->m_Action, $this->m_FullName,
														$this->m_ToAddr) ;
			LogTrans($this->m_Acct, $sTran) ;

			$mailsent = 'T' ;
		}  // if(!$this->Send()) ... else

		// 13 June 2018 - Try to go by whether session # is set or not. 
		//if ($this->m_Sess1 > " " && ($this->m_Action=='1/1'  || $this->m_Action=='1/2' || 
		//							 $this->m_Action=='2/2'  || substr($this->m_Action,0,4)=='Eval') )    
		if ($bInsertSession) 
		{
			// 17 June 2018 Change to db_connect_i() in incDBconn.php
			//                Surprized I didn't do it sooner.
			$bri_conn = db_connect_i('T') ; 
			//db_connect();
			$this->m_NameFirst = $bri_conn->real_escape_string($this->m_NameFirst) ;
			$this->m_NameLast  = $bri_conn->real_escape_string($this->m_NameLast)  ;
			$this->m_ToAddr    = $bri_conn->real_escape_string($this->m_ToAddr)    ;
			$this->m_Rem       = $bri_conn->real_escape_string($this->m_Rem)       ;
			$sQuery = ... 
			
			//$result = mysql_query($query) or die(mysql_error()) ;
			$bri_conn->query($sQuery) ; 
			$bri_conn->close() ; 
		}  // if ($bWriteSessFile)
	
		return $sMsg  ;    // <-- sMsg indicates sucess for failure of e-mail with any error message 
	}  // function SendNow()
	
  function BuildBRImail($inAddr, $inDispName)
	{
		$this->AddAddress($this->m_ToAddr, $this->m_FullName) ;
		$this->SetFrom('No_Reply@baughrelationshipindex.com', 'BRI') ;
		$this->Subject = '???' ;
		if ($this->m_Rt == 'Y')   $this->AddReplyTo($inAddr, $inDispName) ;
		if ($this->m_Bcc == 'Y')  $this->AddBCC($inAddr,     $inDispName) ;

		$this->m_MsgSub = sprintf( 'From %s', $inDispName) ;   // $ufields->dispname ; 
				
		//$this->m_sDebug .= sprintf("<br>Body before chk:={{{%s}}}len:%d=", $this->m_MsgBody, strlen($this->m_MsgBody)) ;  
			
		$retBody  = $this->BodyHeader() ; 
		$retBody .= "Greetings,<br><br>".LF ;
		$retBody .= 'Below is your link for the Baugh Relationship Index (BRI) survey.' ;
		$retBody .= "<br><br>"  ;
		$retBody .= 'The BRI has 52 items and takes about 30 minutes to complete. ' ;
		$retBody .= 'Please click on the link below when you have time.' ;
		$retBody .= "<br><br>"  ;

		if ($this->m_Acct=='39000')
			$sURL  = sprintf('http://www.BaughRelationshipIndex.com/sa-form-5.php?sessid=%s&t=x', $this->m_Sess1 ) ; 
		else
			$sURL  = sprintf('http://www.BaughRelationshipIndex.com/sa-form-4.php?sessid=%s&t=x', $this->m_Sess1 ) ; 
		$retBody .= sprintf('<a href=%s>%s</a><br/>', $sURL, $sURL) ; 
		$retBody .= "<br><br>"  ;
		$retBody .= "Regards,"  ;
		$retBody .= "<br><br>"  ;
		$retBody .= $inDispName ; 
		//$body .= "<br><br><br><br><br><br><br><br><br><br><br><br><br>"  ;
		//$body .= $this->m_Action  ;
		//$bWriteSessFile = true    ;
		$retBody .= $this->BodyFooter() ; 

		return $retBody ; 
	}  // function BuildBRImail()
	
	function BuildNotify($inAddr, $inDispName)
	{
		$this->AddAddress( $inAddr, $inDispName ) ;  // <-- Send to BRI acct holder 
		$this->SetFrom('no-reply@baughrelationshipindex.com', 'Baugh Relationship Index') ;
		$this->m_MsgSub = sprintf('%s has completed the BRI', $this->m_FullName) ; 

		$retBody  = $this->BodyHeader() ; 
		$retBody .= '<br>Log onto BRI site for results' ;
		$retBody .= "<br><br>"  ;
		$sURL = 'http://www.BaughRelationshipIndex.com/index.php' ;
		//$body .= '<a href=' . $sURL . '>' . $sURL . '</a><br>' ; 
		$retBody .= sprintf('<a href=%s>%s</a><br/>', $sURL, $sURL) ; 
		$retBody .= "<br><br><br>"  ; 
		$retBody .= 'Session number:' . $this->m_Sess1 ; 
		$retBody .= "<br><br>"  ; 
		$retBody .= $this->BodyFooter() ; 
		//$bWriteSessFile = false ;
		return $retBody ; 
	} // function BuildNotify()
	
	function BuildSample()
	{
		$this->AddAddress($this->m_ToAddr, $this->m_FullName ) ;
		$this->SetFrom('No_Reply@baughrelationshipindex.com', 'BRI Survey') ;
		$this->m_Prt  = 'Y'  ;
		$this->m_Rem  = $this->m_Action ;    // = "Ref Sample"  ;
		$this->m_Mat  = 'Y'  ;
		//$this->m_Omit = $pOmit ;
		$this->m_MsgSub = sprintf("Sample from %s", $this->m_RefName) ; 

		$retBody = $this->BodyHeader() ; 
		$sPara   = sprintf("%s has sent you a sample Baugh Relationship Index survey", $this->m_RefName)    ;
		$sPara  .= sprintf("&nbsp;and feel free to inquire that this is legitimate.",  $this->m_NameFirst ) ;
		$sPara  .= "&nbsp;Samples are periodically made available to test software." ;  
		$retBody .= sprintf("<p>%s</p>", $sPara) ; 

		$sPara  = "The link below is to the 52-item relationship style questionnaire takes about 30 minutes to complete." ; 
		$sPara .= "&nbsp;When finished you will have an interpretation about 15 pages in length." ; 
		$sPara .= "&nbsp;You will also be asked to give feedback, though it is not required." ;  
		$retBody .= sprintf("<p>%s</p>", $sPara) ; 

		$this->m_Sess1 = rand10() ;  // <-- Get a session number for sample 
		$sURL = sprintf("http://www.BaughRelationshipIndex.com/sa-form-5.php?sessid=%s&t=sample", $this->m_Sess1 ) ; 
		$retBody .= sprintf("<p><a href=%s>%s</a><br/></p>", $sURL, $sURL)   ; 

		$sPara  = "Please save this e-mail. We would like feedback after you review your report" ; 
		$sPara .= "&nbsp;and this link is how you reach our survey." ; 
		$retBody .= sprintf("<p>%s</p>", $sPara) ; 

		$retBody .= "<p>Sincerely<br><br>John V.<br>ldc8286@comcast.net</p>" ; 
		$retBody .= $this->BodyFooter() ; 
		
		return $retBody ; 
	}  // function BuildSample()
	
}   // class JVMailer extends PHPMailer

?>
```
