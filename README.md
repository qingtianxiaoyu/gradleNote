# gradleNote


def doGetHostName(variant) {
    def buildVariants = variant;
    if(variant==null){
        buildVariants = doGetConfigPreference("buildVariants", variant);
    }
    if(buildVariants==null){
        buildVariants="beta"
    }
    def hostName = doGetConfigHostName(buildVariants, "");
    println hostName
    return hostName
}

def doGetConfigXml() {
    def xml = file("config.xml").getText()
    return new XmlParser(false, false).parseText(xml)
}

def doGetConfigPreference(name, defaultValue) {
    name = name.toLowerCase()
    def root = doGetConfigXml()

    def ret = defaultValue
    root.preference.each { it ->
        def attrName = it.attribute("name")
        if (attrName && attrName.toLowerCase() == name) {
            ret = it.attribute("value")
        }
    }
    return ret
}

def doGetConfigHostName(name, defaultValue) {
    name = name.toLowerCase()
    def root = doGetConfigXml()

    def ret = defaultValue
    root.hostName.each { it ->
        def attrName = it.attribute("name")
        if (attrName && attrName.toLowerCase() == name) {
            ret = it.attribute("value")
        }
    }
    return ret
}
