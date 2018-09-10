
## BRI self-admin form check

Make sure test-taker completes all items (most and least), no same most-least responses, etc. The only other requirement is gender and a name.  

```javascript

// These should be global - - - - 
ierrcnt = 0 ; 
serrdesc = "" ;

function itemcheck(itmstr, itemmost, itemleast)  
{
	opmost = -1; 
   	opleast = -1; 
//	Check most - - - - - - -
 	for (i=0;i<=3; i++ ) 
	{
		if (itemmost[i].checked) 
		{
		   opmost = i+1;
		}
	} // for (i=0,i<=3, i++ )

//	Check least - - - - - - -
 	for (i=0;i<=3; i++ ) 
	{
		if (itemleast[i].checked) 
		{
			opleast = i+1;
		}
	} // for (i=0,i<=3, i++ )

//	See what errors there might be	   
	if (opmost <= 0 && opleast <= 0 ) {
	   ierrcnt += 1 ;
	   serrdesc += "  Missing #" + itmstr + "-Most and Least\n" ;
	   }
	  else if (opmost == opleast && opmost > 0) {
	      ierrcnt += 1 ;
	      serrdesc += " #" +itmstr+"-Most and Least are the same \n" ;
          }
	     else if (opmost <= 0) {
	         ierrcnt += 1 ;
	         serrdesc += "  Missing #"+itmstr+"-Most \n" ;
		     }
	        else if (opleast <= 0) {
	            ierrcnt += 1 ;
	            serrdesc += "  Missing #"+itmstr+"-Least \n" ;
		        }
} // function itemcheck() ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^

   
function formcheck() 	
{
	ierrcnt = 0 ; 
	serrdesc = "" ;
	
	if (document.bri_self_admin.nlast1.value.substring(0,2) == "!!")  
	{
		// 31 March 2013 Try to make it easier to enter test records - - - - -
		// Begin last name with !! and the test will be limited 
		if (document.bri_self_admin.nfirst1.value == "")  
		{
		   ierrcnt  += 1 ;
		   serrdesc += "  Missing first name\n" ;
		}

		if (document.bri_self_admin.gender1.value == "")  
		{
		   ierrcnt  += 1 ;
		   serrdesc += "  Missing gender\n" ;
		}
		itemcheck('1',  document.bri_self_admin.itm01m, document.bri_self_admin.itm01l)
		itemcheck('2',  document.bri_self_admin.itm02m, document.bri_self_admin.itm02l)
		itemcheck('3',  document.bri_self_admin.itm03m, document.bri_self_admin.itm03l)
		itemcheck('4',  document.bri_self_admin.itm04m, document.bri_self_admin.itm04l)
	}
	else
	{
		iprcta = parseInt(document.bri_self_admin.aprcnt.value) ;
		iprctb = parseInt(document.bri_self_admin.bprcnt.value) ;
		iprctc = parseInt(document.bri_self_admin.cprcnt.value) ;
		// 9 June 07 Add partner's percentage check 
		iprcttot = iprcta+iprctb+iprctc ;
		iprcta2 = parseInt(document.bri_self_admin.aprcnt2.value) ;
		iprctb2 = parseInt(document.bri_self_admin.bprcnt2.value) ;
		iprctc2 = parseInt(document.bri_self_admin.cprcnt2.value) ;
		iprcttot2 = iprcta2+iprctb2+iprctc2 ;
	
		if (document.bri_self_admin.nfirst1.value == "")  
		{
		   ierrcnt  += 1 ;
		   serrdesc += "  Missing first name\n" ;
		}

		if (document.bri_self_admin.nlast1.value == "")  
		{
			ierrcnt  += 1 ;
	   		serrdesc += "  Missing last name\n" ;
		}

		if (document.bri_self_admin.gender1.value == "")  
		{
		   ierrcnt  += 1 ;
		   serrdesc += "  Missing gender\n" ;
		}

		itemcheck('1',  document.bri_self_admin.itm01m, document.bri_self_admin.itm01l)
		itemcheck('2',  document.bri_self_admin.itm02m, document.bri_self_admin.itm02l)
		itemcheck('3',  document.bri_self_admin.itm03m, document.bri_self_admin.itm03l)
		itemcheck('4',  document.bri_self_admin.itm04m, document.bri_self_admin.itm04l)
		itemcheck('5',  document.bri_self_admin.itm05m, document.bri_self_admin.itm05l)
		itemcheck('6',  document.bri_self_admin.itm06m, document.bri_self_admin.itm06l)
		itemcheck('7',  document.bri_self_admin.itm07m, document.bri_self_admin.itm07l)
		itemcheck('8',  document.bri_self_admin.itm08m, document.bri_self_admin.itm08l)
		itemcheck('9',  document.bri_self_admin.itm09m, document.bri_self_admin.itm09l)
		itemcheck('10', document.bri_self_admin.itm10m, document.bri_self_admin.itm10l)
		itemcheck('11', document.bri_self_admin.itm11m, document.bri_self_admin.itm11l)
		itemcheck('12', document.bri_self_admin.itm12m, document.bri_self_admin.itm12l)
		itemcheck('13', document.bri_self_admin.itm13m, document.bri_self_admin.itm13l)
		itemcheck('14', document.bri_self_admin.itm14m, document.bri_self_admin.itm14l)
		itemcheck('15', document.bri_self_admin.itm15m, document.bri_self_admin.itm15l)
		itemcheck('16', document.bri_self_admin.itm16m, document.bri_self_admin.itm16l)
		itemcheck('17', document.bri_self_admin.itm17m, document.bri_self_admin.itm17l)
		itemcheck('18', document.bri_self_admin.itm18m, document.bri_self_admin.itm18l)
		itemcheck('19', document.bri_self_admin.itm19m, document.bri_self_admin.itm19l)
		itemcheck('20', document.bri_self_admin.itm20m, document.bri_self_admin.itm20l)
		itemcheck('21', document.bri_self_admin.itm21m, document.bri_self_admin.itm21l)
		itemcheck('22', document.bri_self_admin.itm22m, document.bri_self_admin.itm22l)
		itemcheck('23', document.bri_self_admin.itm23m, document.bri_self_admin.itm23l)
		itemcheck('24', document.bri_self_admin.itm24m, document.bri_self_admin.itm24l)
		itemcheck('25', document.bri_self_admin.itm25m, document.bri_self_admin.itm25l)
		itemcheck('26', document.bri_self_admin.itm26m, document.bri_self_admin.itm26l)
		itemcheck('27', document.bri_self_admin.itm27m, document.bri_self_admin.itm27l)
		itemcheck('28', document.bri_self_admin.itm28m, document.bri_self_admin.itm28l)
		itemcheck('29', document.bri_self_admin.itm29m, document.bri_self_admin.itm29l)
		itemcheck('30', document.bri_self_admin.itm30m, document.bri_self_admin.itm30l)
		itemcheck('31', document.bri_self_admin.itm31m, document.bri_self_admin.itm31l)
		itemcheck('32', document.bri_self_admin.itm32m, document.bri_self_admin.itm32l)
		itemcheck('33', document.bri_self_admin.itm33m, document.bri_self_admin.itm33l)
		itemcheck('34', document.bri_self_admin.itm34m, document.bri_self_admin.itm34l)
		itemcheck('35', document.bri_self_admin.itm35m, document.bri_self_admin.itm35l)
		itemcheck('36', document.bri_self_admin.itm36m, document.bri_self_admin.itm36l)
		itemcheck('37', document.bri_self_admin.itm37m, document.bri_self_admin.itm37l)
		itemcheck('38', document.bri_self_admin.itm38m, document.bri_self_admin.itm38l)
		itemcheck('39', document.bri_self_admin.itm39m, document.bri_self_admin.itm39l)
		itemcheck('40', document.bri_self_admin.itm40m, document.bri_self_admin.itm40l)
		itemcheck('41', document.bri_self_admin.itm41m, document.bri_self_admin.itm41l)
		itemcheck('42', document.bri_self_admin.itm42m, document.bri_self_admin.itm42l)
		itemcheck('43', document.bri_self_admin.itm43m, document.bri_self_admin.itm43l)
		itemcheck('44', document.bri_self_admin.itm44m, document.bri_self_admin.itm44l) 
		itemcheck('45', document.bri_self_admin.itm45m, document.bri_self_admin.itm45l)
		itemcheck('46', document.bri_self_admin.itm46m, document.bri_self_admin.itm46l)
		itemcheck('47', document.bri_self_admin.itm47m, document.bri_self_admin.itm47l)
		itemcheck('48', document.bri_self_admin.itm48m, document.bri_self_admin.itm48l)
		itemcheck('49', document.bri_self_admin.itm49m, document.bri_self_admin.itm49l)

		if (document.bri_self_admin.bmat.value == 1) 
		{
			if (iprcttot != 100) 
			{
			   ierrcnt  += 1 ;
			   serrdesc += "  Your own maturity assessment in Section 2 does not add up to 100% (total is "+iprcttot+"%)\n" ;
			}
		} // (document.bri_self_admin.bmat.value == 1)

		if (document.bri_self_admin.omit.value == 'N') 
		{
			if (iprcttot2 != 100) 
			{
			   ierrcnt  += 1 ;
			   serrdesc += "  Your assessment of partner's maturity in Section 2 does not add up to 100% (total is "+iprcttot2+"%)\n" ;
			}
		} // (document.bri_self_admin.omit.value == 'N')
		  
		  
	}  // if (document.bri_self_admin.nlast1.value.substring(0,2) == "!!")   ELSE
		  	  
//  Show error detail for whole page, if any - - - - - 
	if (ierrcnt > 0)  
	{
	   if (ierrcnt == 1)
			alert("Your survey BRI has " + ierrcnt + " error\n" + serrdesc );
		  else
			alert("Your survey BRI has " + ierrcnt + " errors\n" + serrdesc );
	   return false;
	}
   
} // function formCheck()

```
