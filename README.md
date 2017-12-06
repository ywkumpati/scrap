find xpath online
http://xmltoolbox.appspot.com/xpath_generator.html

soapUI groovy script 

def verCode = UUID.randomUUID().toString()


def respXml = testRunner.testCase.testSteps["getAccountsForPerson"].testRequest.response.getResponseContent()

def respXmlObj = new XmlHolder(respXml)
def groovyUtils = new com.eviware.soapui.support.GroovyUtils( context )
groovyUtils.setPropertyValue("Properties", "guid", verCode)

def keys = respXmlObj.getNodeValues("//*:AccountList/*:Account/key/id")
def active = respXmlObj.getNodeValues("//*:AccountList/*:Account/active")
def typeOfPayment = respXmlObj.getNodeValues("//*:AccountList/*:Account/type")

for (i = 0; i &lt; active.length; i++) {
	def type = respXmlObj.getNodeValues("//AccountList//Account["+(i+1)+"]/@*:type")
	def alias = respXmlObj.getNodeValues("//AccountList//Account["+(i+1)+"]/alias")
	log.info(alias.toString())
   if("bankAccount".equals(type[0]) &amp;&amp; "true".equals(active[i])){
   	 groovyUtils.setPropertyValue("Properties", "propAccountId", keys[i])
   	 groovyUtils.setPropertyValue("Properties", "typeOfPayment", typeOfPayment[i])
   	 groovyUtils.setPropertyValue("Properties", "aliasOfBank", alias[0])
   	 break;
   	}
}

