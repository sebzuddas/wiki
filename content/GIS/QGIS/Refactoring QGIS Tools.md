
Previous Tool Format

```
from qgis.core import QgsProcessing

from qgis.core import QgsProcessingAlgorithm

from qgis.core import QgsProcessingMultiStepFeedback

from qgis.core import QgsProcessingParameterVectorLayer

from qgis.core import QgsProcessingFeatureSource

from qgis.core import QgsProcessingParameterFeatureSource

from qgis.core import QgsProcessingParameterEnum

from qgis.core import QgsProcessingParameterString

from qgis.core import QgsProcessingParameterBoolean

from qgis.core import QgsMessageLogConsole

from qgis.core import QgsVectorLayer

from qgis.PyQt.QtCore import QCoreApplication

from qgis.core import QgsMessageLog

from qgis.utils import iface

from qgis.core import QgsProject

from qgis.core import QgsProject

import re, sys, traceback, os

import processing

import psycopg2

  

rasterGeoStore = r"\\SQL-GIS01\Raster_GeoStore"


def tr(string):

    return QCoreApplication.translate("Processing", string)


def excep():

    tb = sys.exc_info()[2]

    tbinfo = traceback.format_tb(tb)[0]

    errorM = str(sys.exc_info()[1]).split("\n")[0]

    if "," in errorM:

        errorM = errorM.split(",")[0] + ". " + errorM.split(",")[1]

    lineN = str(tbinfo).split(", ")[1]

    mess = "Error  [" + lineN + "] " + errorM

    return mess

  
  

classToSchema = {

    "o1_agriculture_aquaculture": "Agriculture and Aquaculture",

    "o2_boundary": "Boundary",

    "o3_ict": "Information and Communication Technologies",

    "o4_demographic": "Demographic",

    "o5_education": "Education",

    "o6_energy": "Energy",

    "o7_environment": "Environment",

    "o8_health": "Health",

    "o9_hydrology": "Hydrology",

    "o10_landuse": "Landuse",

    "o11_marine": "Marine",

    "o12_poi": "Point of Interest ",

    "o13_raster": "Raster",

    "o14_security_safety": "Security and Safety",

    "o15_survey": "Survey",

    "o16_topographic": "Topographic",

    "o17_transportation": "Transportation",

    "o18_utilities": "Utilities",

}

  

schemaClassD = {

    "aat": "o1_agriculture_aquaculture",

    "bdy": "o2_boundary",

    "ict": "o3_ict",

    "dgp": "o4_demographic",

    "edu": "o5_education",

    "eng": "o6_energy",

    "env": "o7_environment",

    "hth": "o8_health",

    "hdy": "o9_hydrology",

    "lsu": "o10_landuse",

    "mrn": "o11_marine",

    "poi": "o12_poi",

    "ras": "o13_raster",

    "sas": "o14_security_safety",

    "srv": "o15_survey",

    "tpg": "o16_topographic",

    "trs": "o17_transportation",

    "utl": "o18_utilities",

}

  

schemaList = [

    "o1_agriculture_aquaculture",

    "o2_boundary",

    "o3_ict",

    "o4_demographic",

    "o5_education",

    "o6_energy",

    "o7_environment",

    "o8_health",

    "o9_hydrology",

    "o10_landuse",

    "o11_marine",

    "o12_poi",

    "o13_raster",

    "o14_security_safety",

    "o15_survey",

    "o16_topographic",

    "o17_transportation",

    "o18_utilities",

    "oo4_gis_data_catalogue",

    "oo5_urban_models",

    "OO6_original_data",

    "OO7_archived_data",

]

  
  

def getConnections():

    userDBConns = {}

    try:

        try:

            from PyQt5.QtCore import QSettings

        except:

            try:

                from PyQt4.QtCore import QSettings

            except:

                QgsMessageLog.logMessage(

                    message=str(

                        "Cannot get PostGIS connections. {}".format(excep()),

                        tag="BH- Execute Privileges",

                        level=1,

                    )

                )

        userDBConnections = []

        s = QSettings()

        s.beginGroup("PostgreSQL/connections")

        for k in s.allKeys():

            if "database" in k:

                userP = s.value(k.replace("database", "username"))

                passP = s.value(k.replace("database", "password"))

                userDBConns.update({k.split("/")[0]: [s.value(k), userP, passP]})

                # QgsMessageLog.logMessage(message=str("{}, {}, {}".format(s.value(k),userP,passP)), tag='BH- Execute Privileges', level=1)

        s.endGroup()

        # databaseList = [details[0] + " ("+ db+")" for db,details in userDBConns.items()]

        databaseList = [

            connName + " [" + details[0] + "]"

            for connName, details in userDBConns.items()

        ]

        return userDBConns, databaseList

    except:

        QgsMessageLog.logMessage(

            message=str("Cannot get PostGIS connections. {}".format(excep())),

            tag="BH- Execute Privileges",

            level=1,

        )

        databaseList = [tr("ksa"), tr("uk"), tr("chl"), tr("rou")]

hostL = [

    "sql-gis01",

    "sql-stage-gis01",

    "SQL-AZR-GIS01.burohappold.com",

    "SQL-AZRST-GIS01.burohappold.com",

]

roleL = ["bhgisdba", "bhgiseditor", "bhgisreader", "bhgisguest"]

  
  

def cleanName(layername):

    if "." in layername:

        layername = layername.split(".")[1]

    if " " in layername:

        layername = layername.split(" ")[1]

    for rem in ["_point", "_line", "_polygon"]:

        if rem in layername:

            layername = layername.replace(rem, "")

    layername = (

        layername.replace("-", "")

        .replace("(", "")

        .replace(")", "")

        .replace(" ", "")

        .replace(".", "")

    )

    return layername.strip()

  
  

def getRoles(hostP, userP, passP):

    try:

        connection = psycopg2.connect(

            user=userP, password=passP, host=hostP, port="5432", database="postgres"

        )

        cursor = connection.cursor()

        query01 = "SELECT rolname FROM pg_roles WHERE rolcanlogin = False AND rolname NOT LIKE 'pg_%';"

        rolesL = []

        try:

            cursor.execute(query01)

            records = cursor.fetchall()

            # QgsMessageLog.logMessage(message=str(records), tag='BH- Execute Privileges', level=1)

            for rec in records:

                rolesL.append(rec[0])

        except (Exception, psycopg2.Error) as error:

            mes = "Error while fetching roles PostgreSQL: " + str(error) + excep()

            QgsMessageLog.logMessage(

                message=str(mes), tag="BH- Execute Privileges", level=1

            )

        cursor.close()

        connection.close()

        rolesL.sort()

        # QgsMessageLog.logMessage(message=str(rolesL), tag='BH- Execute Privileges', level=1)

        return rolesL

  

    except (Exception, psycopg2.Error) as error:

        mes = "Error while fetching roles PostgreSQL: " + str(error) + excep()

        QgsMessageLog.logMessage(

            message=str(mes), tag="BH- Execute Privileges", level=1

        )

  
  

def getDatabases(hostP, userP, passP):

    databaseL = []

    try:

        connection = psycopg2.connect(

            user=userP, password=passP, host=hostP, port="5432", database="postgres"

        )

        connection.autocommit = True

        cursor = connection.cursor()

  

        #  AND datname NOT LIKE 'bh_gis%'

        query00 = "SELECT datname FROM pg_database WHERE datname NOT LIKE 'template%' AND datname NOT LIKE 'post%';"

        cursor.execute(query00)

        records = cursor.fetchall()

        for rec in records:

            databaseL.append(rec[0])

            # Dict.update({rec[1]:rec[0]})

        cursor.close()

        connection.close()

        return databaseL

  

    except (Exception, psycopg2.Error) as error:

        mes = "Error getting list of databases: " + str(error) + excep()

        QgsMessageLog.logMessage(message=str(mes), tag="BH- Create User", level=1)

  
  

def scanDatabase(dbP, hostP, userP, passP):

    ### Get dataset naming info from postgres tables

    tableList = []

    tableDict = {}

    tableSize = {}

    rasterDict = {}

  

    try:

        import psycopg2

  

        connection = psycopg2.connect(

            user=userP, password=passP, host=hostP, port="5432", database=dbP

        )

        cursor = connection.cursor()

  

        query01 = "SELECT schemaname,relname,n_live_tup FROM pg_stat_user_tables WHERE schemaname NOT IN ('tiger','topology','public')"

        query02 = (

            "SELECT r_table_schema,r_table_name,r_raster_column FROM raster_columns"

        )

        # QgsMessageLog.logMessage(message=str(query01), tag='BH- Execute Privileges', level=1)

  

        cursor.execute(query01)

        records = cursor.fetchall()

        # QgsMessageLog.logMessage(message=str(records), tag='BH- Execute Privileges', level=1)

        for rec in records:

            schema = rec[0]

            table = rec[1]

            records = rec[2]

            tableList.append(table)

            # projDict2.update({key : val})

            tableDict.update({table: schema})

            tableSize.update({table: records})

  

        cursor.execute(query02)

        records = cursor.fetchall()

        # QgsMessageLog.logMessage(message=str(records), tag='BH- Execute Privileges', level=1)

        for rec in records:

            rasterDict.update({rec[1]: rec[0]})

  

        cursor.close()

        connection.close()

  

        # QgsMessageLog.logMessage(message="tableDict: {}".format(tableDict), tag='BH- Execute Privileges', level=1)

  

    except (Exception, psycopg2.Error) as error:

        mes = ("Error while fetching data from PostgreSQL", error)

        QgsMessageLog.logMessage(

            message="Error: {}".format(mes), tag="BH- Execute Privileges", level=1

        )

  

    # tableList = [v for k,v in projDict2.items()]

    tableList.sort()

    return tableList, tableDict, tableSize

  
  

class DatabaseReport(QgsProcessingAlgorithm):

    DatabaseChoice = "DatabaseChoice"

    databaseName = "databaseName"

    bool01 = "bool01"

    serverChoice = "serverChoice"

    stringSearch = "stringSearch"

    schemChoice = "schemChoice"

    exportPath = "exportPath"

    roleChoice = "roleChoice"

    mess = "mess"

  

    def __init__(self):

        super().__init__()

  

    def createInstance(self):

        return type(self)()

  

    def initAlgorithm(self, config=None):

        try:

            userP = None

            try:

                userDBConns, databaseList = getConnections()

                firstConn = list(userDBConns.keys())[0]

                firstVals = userDBConns.get(firstConn)

                userP = firstVals[1]

                passP = firstVals[2]

            except:

                QgsMessageLog.logMessage(

                    message=str(

                        "NO POSTGIS CONNECTIONS SETUP YET, please add one and re-run tool"

                    ),

                    tag="BH- Execute Privileges",

                    level=1,

                )

  

            try:

                hostP = "sql-gis01"

                rolesL = getRoles(hostP, userP, passP)

                # model_feedback.pushInfo(str(rolesL))

            except:

                QgsMessageLog.logMessage(

                    message=str(

                        "Cannot get roles. User: {} Password: {}".format(userP, passP)

                    ),

                    tag="BH- Execute Privileges",

                    level=1,

                )

                rolesL = ["bhgisreader", "bhgiseditor"]

  

            try:

                databaseL = getDatabases(hostL[0], userP, passP)

            except:

                QgsMessageLog.logMessage(

                    message=str("Cannot get dynamic database list. "),

                    tag="BH- Execute Privileges",

                    level=1,

                )

                databaseL = [

                    "ksa",

                    "uk",

                    "chl",

                    "rou",

                    "deu",

                    "ind",

                    "usa",

                    "indonesia",

                    "global",

                ]

  

            if userP != None:

                self.addParameter(

                    QgsProcessingParameterEnum(

                        self.serverChoice,

                        tr("Choose Server to apply permissions to"),

                        options=hostL,

                        defaultValue=0,

                        optional=True,

                    )

                )

  

                self.addParameter(

                    QgsProcessingParameterEnum(

                        self.DatabaseChoice,

                        tr("Choose Database from list"),

                        options=databaseL,

                        defaultValue=None,

                        optional=True,

                    )

                )

  

                self.addParameter(

                    QgsProcessingParameterString(

                        self.databaseName,

                        tr("Or type new database name here"),

                        defaultValue="",

                        optional=True,

                    )

                )

  

                self.addParameter(

                    QgsProcessingParameterString(

                        self.schemChoice,

                        tr(

                            "Type/paste in schema name (leave blank to apply to standard BH schemas)"

                        ),

                        defaultValue="",

                        optional=True,

                    )

                )

  

                self.addParameter(

                    QgsProcessingParameterEnum(

                        self.roleChoice,

                        tr("Choose role or user to apply permission to"),

                        options=rolesL,

                        allowMultiple=True,

                        defaultValue=None,

                        optional=True,

                    )

                )

  

                self.addParameter(

                    QgsProcessingParameterBoolean(

                        self.bool01,

                        tr("Apply permissions for all roles in list above"),

                        defaultValue=False,

                    )

                )

            else:

                databaseList = []

                self.addParameter(

                    QgsProcessingParameterEnum(

                        self.DatabaseChoice,

                        tr(

                            "ERROR: PLEASE SETUP POSTGIS CONNECTION(S) FIRST (or delete and recreate connection) - then re-run tool. You also need Cisco VPN connection to BH Network)"

                        ),

                        options=databaseList,

                        defaultValue=None,

                        optional=False,

                    )

                )

  

        except:

            QgsMessageLog.logMessage(

                message=str(

                    "Cannot connect to database to get table information. Is wifi on and cisco connected to BH network?"

                ),

                tag="BH- Execute Privileges",

                level=1,

            )

  

    def processAlgorithm(self, parameters, context, model_feedback):

        errors = []

        results = {}

        outputs = {}

        missingVals = []

        reportL = []

        resultFile = None

  

        try:

            userDBConns, databaseList = getConnections()

  

            databaseChoiceP = self.parameterAsString(

                parameters, self.DatabaseChoice, context

            )

            databaseNameP = self.parameterAsString(

                parameters, self.databaseName, context

            )

            serverP = self.parameterAsString(parameters, self.serverChoice, context)

            # stringSearchP = self.parameterAsString(parameters,self.stringSearch,context)

            # exportPathP = self.parameterAsString(parameters,self.exportPath,context)

            rolePar = self.parameterAsEnums(parameters, self.roleChoice, context)

            bool01Ch = self.parameterAsBool(parameters, self.bool01, context)

            schemP = self.parameterAsString(parameters, self.schemChoice, context)

  

            schemP = schemP.strip()

            if len(schemP) > 0:

                schemaList = []

                schemaList.append(schemP)

            else:

                schemaList = [

                    "o1_agriculture_aquaculture",

                    "o2_boundary",

                    "o3_ict",

                    "o4_demographic",

                    "o5_education",

                    "o6_energy",

                    "o7_environment",

                    "o8_health",

                    "o9_hydrology",

                    "o10_landuse",

                    "o11_marine",

                    "o12_poi",

                    "o13_raster",

                    "o14_security_safety",

                    "o15_survey",

                    "o16_topographic",

                    "o17_transportation",

                    "o18_utilities",

                    "oo4_gis_data_catalogue",

                    "oo5_urban_models",

                    "oo6_original_data",

                    "oo7_archived_data",

                ]

  

            model_feedback.pushInfo("ServerP: {}".format(serverP))

            model_feedback.pushInfo("hostL: {}".format(hostL))

            hostP = hostL[int(serverP)]

            userP = "postgres"

            model_feedback.pushInfo(str(hostP))

  

            if hostP == "sql-gis01":

                passP = "bhgis"

            elif hostP == "sql-stage-gis01":

                passP = "bgis"

            elif hostP == "SQL-AZR-GIS01.burohappold.com":

                passP = "bhgis25"

            elif hostP == "SQL-AZRST-GIS01.burohappold.com":

                passP = "bhgis25"

            else:

                passP = None

  

            rolesL = getRoles(hostP, userP, passP)

            roleChoiceL = []

            if len(rolePar) != 0:

                for role in rolePar:

                    rolePar = rolesL[int(role)]

                    roleChoiceL.append(rolePar)

  

            model_feedback.pushInfo("Chosen roles: {}".format(",".join(roleChoiceL)))

  

            if passP != None:

                try:

                    databaseL = getDatabases(hostL[0], userP, "bhgis")

                    # databaseL = getDatabases(hostP,userP,passP)

                except:

                    QgsMessageLog.logMessage(

                        message=str("Cannot get dynamic database list. "),

                        tag="BH- Execute Privileges",

                        level=1,

                    )

                    databaseL = databaseList

                #

                # try:

                #    rolesL = getRoles(hostP,userP,passP)

                #    model_feedback.pushInfo(str(rolesL))

                # except:

                #    model_feedback.pushInfo("\Cannot get list of roles, defaulting to basic list")

                #    rolesL = ['bhgisreader','bhgiseditor']

  

                # if rolePar != None and rolePar != "":

                #    #model_feedback.pushInfo("'{}'".format(rolePar))

                #    roleX = rolesL[int(rolePar)]

                # else:

                #    roleX = None

  

                if databaseChoiceP == None or len(databaseChoiceP) < 1:

                    if len(databaseNameP) > 0:

                        databaseCh = databaseNameP

                    else:

                        model_feedback.pushInfo(

                            "\nYOU MUST SELECT A DATABASE FIRST or type in a databasename if not listed\n"

                        )

                else:

                    model_feedback.pushInfo(

                        "Applying permissions to server: '{}'".format(hostP)

                    )  # roleX

                    # dbP = databaseList[int(databaseChoiceP)]

                    # connCh = dbP.split(" [")[0]

                    # databaseCh = dbP.split(" [")[1].split("]")[0]

                    # databaseP = databaseCh

  

                    databaseCh = databaseL[int(databaseChoiceP)]

                    # databaseCh = "chl" # overwrite choice if live and stage don't match

                    model_feedback.pushInfo("\nChosen host: '{}'".format(hostP))

                    model_feedback.pushInfo(

                        "Chosen Database: '{}'\n".format(databaseCh)

                    )

                    # dictVal = [val for connName,val in userDBConns.items() if connName == connCh][0]

                    # dbP2 = databaseCh

  

                    # userP = dictVal[1]

                    # passP = dictVal[2]

                    # connName = [conn for conn,details in userDBConns.items() if databaseCh == details[0]][0]

                    # model_feedback.pushInfo("\nUser: {} - Pass: {}".format(userP, passP))

                    # model_feedback.pushInfo("\nUser: {}".format(userP))

  

                bhgisdba = [

                    "GRANT USAGE, CREATE ON SCHEMA schemaP TO bhgisdba;",

                    "GRANT ALL ON ALL TABLES IN SCHEMA schemaP TO bhgisdba WITH GRANT OPTION;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT INSERT, SELECT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER ON TABLES TO bhgisdba;",

                    "ALTER DEFAULT PRIVILEGES FOR Role bhgisdba GRANT USAGE, SELECT, UPDATE ON SEQUENCES TO bhgisdba;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT EXECUTE ON FUNCTIONS TO bhgisdba;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT USAGE ON TYPES TO bhgisdba;",

                ]

  

                bhgiseditor = [

                    "GRANT USAGE, CREATE ON SCHEMA schemaP TO bhgiseditor;",

                    "GRANT ALL ON ALL TABLES IN SCHEMA schemaP TO bhgiseditor WITH GRANT OPTION;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT INSERT, SELECT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER ON TABLES TO bhgiseditor;",

                    "ALTER DEFAULT PRIVILEGES FOR Role bhgiseditor GRANT USAGE, SELECT, UPDATE ON SEQUENCES TO bhgiseditor;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT EXECUTE ON FUNCTIONS TO bhgiseditor;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT USAGE ON TYPES TO bhgiseditor;",

                ]

  

                bhgisreader = [

                    "GRANT USAGE ON SCHEMA schemaP TO bhgisreader;",

                    "GRANT SELECT ON ALL TABLES IN SCHEMA schemaP TO bhgisreader WITH GRANT OPTION;",

                    "ALTER DEFAULT PRIVILEGES FOR USER bhgisreader IN SCHEMA schemaP GRANT SELECT ON SEQUENCES TO bhgisreader;",

                    "ALTER DEFAULT PRIVILEGES FOR USER bhgisreader IN SCHEMA schemaP GRANT SELECT ON TABLES TO bhgisreader;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT SELECT, REFERENCES, TRIGGER ON TABLES TO bhgisreader;",

                    "ALTER DEFAULT PRIVILEGES FOR Role bhgisreader GRANT USAGE, SELECT ON SEQUENCES TO bhgisreader;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT EXECUTE ON FUNCTIONS TO bhgisreader;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT USAGE ON TYPES TO bhgisreader;",

                ]

  

                bhgisguest = [

                    "GRANT USAGE ON SCHEMA schemaP TO bhgisguest;",

                    "GRANT SELECT ON ALL TABLES IN SCHEMA schemaP TO bhgisguest WITH GRANT OPTION;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT SELECT, REFERENCES, TRIGGER ON TABLES TO bhgisguest;",

                    "ALTER DEFAULT PRIVILEGES FOR Role bhgisguest GRANT USAGE, SELECT ON SEQUENCES TO bhgisguest;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT EXECUTE ON FUNCTIONS TO bhgisguest;",

                    "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT USAGE ON TYPES TO bhgisguest;",

                ]

  

                queriesL = []  # will contain all groups of role permissions to apply

  

                # QueriesL = ["GRANT INSERT, SELECT, UPDATE ON SCHEMA schemaP TO roleP;",

                #            "GRANT ALL ON ALL TABLES IN SCHEMA schemaP TO roleP WITH GRANT OPTION;",

                #            "ALTER DEFAULT PRIVILEGES IN SCHEMA schemaP GRANT INSERT, SELECT, UPDATE, REFERENCES, TRIGGER ON TABLES TO roleP;" ]

  

                if bool01Ch == True:

                    model_feedback.pushInfo(

                        "\nChosen to apply ALL role permissions to the database: '{}'".format(

                            databaseCh

                        )

                    )

                    queriesL.extend([bhgisdba, bhgiseditor, bhgisreader, bhgisguest])

  

                    for rol in rolesL:

                        if "editor" in rol:

                            new_role_sql = [

                                s.replace("bhgiseditor", rol) for s in bhgiseditor

                            ]

                        elif "reader" in rol:

                            new_role_sql = [

                                s.replace("bhgisreader", rol) for s in bhgisreader

                            ]

                        elif "dba" in rol:

                            new_role_sql = [

                                s.replace("bhgisdba", rol) for s in bhgisdba

                            ]

                        else:

                            new_role_sql = []

                        queriesL.append(new_role_sql)

  

                else:

                    for roleX in roleChoiceL:

                        model_feedback.pushInfo(

                            "\nChosen to apply role permissions for '{}' to the database: '{}'".format(

                                roleX, databaseCh

                            )

                        )

                        if "editor" in roleX:

                            new_role_sql = [

                                s.replace("bhgiseditor", roleX) for s in bhgiseditor

                            ]

                        elif "reader" in roleX:

                            new_role_sql = [

                                s.replace("bhgisreader", roleX) for s in bhgisreader

                            ]

                        elif "dba" in roleX:

                            new_role_sql = [

                                s.replace("bhgisdba", roleX) for s in bhgisdba

                            ]

  

                        queriesL.append(new_role_sql)

                    # model_feedback.pushInfo("QueryList: {}".format(eval(roleX)))

  

                # model_feedback.pushInfo("QueryList: {}".format(queriesL))

                for qL in queriesL:

                    # model_feedback.pushInfo("QueryList: {}".format(qL[0].split("TO ")[1].replace(";","")))

                    model_feedback.pushInfo(

                        "Aplying permissions for role: {} to '{}'".format(

                            qL, databaseCh

                        )

                    )

                    try:

                        import psycopg2

  

                        connection = psycopg2.connect(

                            user=userP,

                            password=passP,

                            host=hostP,

                            port="5432",

                            database=databaseCh,

                        )

                        connection.autocommit = True

                        cursor = connection.cursor()

  

                        for sch in schemaList:

                            # model_feedback.pushInfo("Schema: {}".format(sch))

                            for q in qL:

                                if roleX != None:

                                    q = q.replace("schemaP", sch).replace(

                                        "roleP", roleX

                                    )

                                else:

                                    q = q.replace("schemaP", sch)

                                # model_feedback.pushInfo(q)

  

                                try:

                                    cursor.execute(q)

                                    connection.commit()

  

                                    model_feedback.pushInfo(

                                        "Executed query OK: '{}'".format(q)

                                    )

                                except:

                                    model_feedback.pushInfo(

                                        "\nFailed to execute query:\n{}\n{}".format(

                                            q, excep()

                                        )

                                    )

  

                    except:

                        # model_feedback.pushInfo("\nError")

                        model_feedback.pushInfo(excep())

  

                ### Finally revoke privileges to sensitive datasets

                try:

                    revokeStringL = ["kaust_"]

                    acceptedRoles = ["kaust_reader", "bhdba", "postgres", "bhgisdba"]

                    revokeRoles = [

                        r for r in rolesL if databaseCh in r and r not in acceptedRoles

                    ]

                    revokeRoles.extend(["bhgiseditor", "bhgisguest", "bhgisreader"])

                    model_feedback.pushInfo(

                        "Roles to remain: '{}'".format(acceptedRoles)

                    )

                    model_feedback.pushInfo("Roles to revoke: '{}'".format(revokeRoles))

                    tableList, tableDict, tableSize = scanDatabase(

                        databaseCh, hostP, userP, passP

                    )

                    model_feedback.pushInfo("Tables: '{}'".format(tableDict))

                    revisedDict = {

                        tb: sc

                        for tb, sc in tableDict.items()

                        if len([rv for rv in revokeStringL if rv in tb.lower()]) > 0

                    }

                    model_feedback.pushInfo(

                        "Tables to revoke: '{}'".format(revisedDict)

                    )

                    revokeQueryL = []

                    # permissionL = ["SELECT","INSERT","UPDATE","DELETE","TRUNCATE","REFERENCES","TRIGGER"]

                    for table, schema in revisedDict.items():

                        # for permissionP in permissionL:

                        for roleP in revokeRoles:

                            revokeQueryL.append(

                                "REVOKE ALL PRIVILEGES ON {}.{} FROM {}".format(

                                    schema, table, roleP

                                )

                            )

  

                    for q in revokeQueryL:

                        try:

                            cursor.execute(q)

                            connection.commit()

                            model_feedback.pushInfo("Executed query OK: '{}'".format(q))

                        except:

                            model_feedback.pushInfo(

                                "\nFailed to execute query:\n{}\n{}".format(q, excep())

                            )

                    cursor.close()

                    connection.close()

                except:

                    model_feedback.pushInfo(excep())

  

            else:

                model_feedback.pushInfo(

                    "Cannot get password for Server Choice: {}".format(hostP)

                )

        except:

            # model_feedback.pushInfo("\nError")

            model_feedback.pushInfo(excep())

  

        return results

  

    def name(self):

        return "DBA01.1 Execute Permissions to Database"

  

    def displayName(self):

        return "DBA01.1 Execute Permissions to Database"

  

    def group(self):

        return "DBA"

  

    def groupId(self):

        return "DBA"

  

    def shortHelpString(self):

        mes = """

        WARNING: If tool is blank, please setup at least 1 PostGIS connection, or delete and recreate the connection

        This tool allows DBAs to apply default privileges for servers,. database, for all BH schemas for some selected roles.

        """

        return tr(mes)

  

    def tr(string):

        return QCoreApplication.translate(string)

  

    def createInstance(self):

        return DatabaseReport()

```




---

new structure
```
from qgis.core import QgsProcessing

from qgis.core import QgsProcessingAlgorithm

from qgis.core import QgsProcessingMultiStepFeedback

from qgis.core import QgsProcessingParameterVectorLayer

from qgis.core import QgsProcessingFeatureSource

from qgis.core import QgsProcessingParameterFeatureSource

from qgis.core import QgsProcessingParameterFolderDestination

from qgis.core import QgsProcessingParameterEnum

from qgis.core import QgsProcessingParameterString

from qgis.core import QgsProcessingParameterBoolean

from qgis.core import QgsMessageLogConsole

from qgis.core import QgsVectorLayer

from qgis.core import QgsFeature

from qgis.PyQt.QtCore import QCoreApplication

from qgis.core import QgsMessageLog

from qgis.utils import iface

from qgis.core import QgsVectorLayer

from qgis.core import QgsRasterLayer

from qgis.core import QgsProject

from qgis.core import QgsGeometry

from qgis.core import QgsPoint

from qgis.core import QgsPolygon

from qgis.core import QgsCoordinateReferenceSystem

from qgis.core import QgsCoordinateTransform

from qgis.core import QgsDataSourceUri

from qgis.core import QgsVectorFileWriter

import re, sys, traceback, os

import shutil

import json

import psycopg2

import processing

from datetime import datetime, date

  

class CleanColumns(QgsProcessingAlgorithm):

    serverChoice = "serverChoice"

    DatabaseChoice = "DatabaseChoice"

    databaseConnection = "databaseConnection"

    filterString = "filterString"

    userChoiceM = "userChoiceM"

    userP = "bheditor"

    passP = "bhcreator25"

    rasterGeoStore = r"\\SQL-GIS01\Raster_GeoStore"

    geoPack = r"\\SQL-GIS01\GIS_Admin\Templates\EmptyGeopackage.gpkg"

    exportFolder = "exportFolder"

    mess = "mess"

    hostL = ["sql-gis01", "sql-stage-gis01"]

    validRasters = [

    ".ecw",

    ".tif",

    ".tiff",

    ".png",

    ".bmp",

    ".sid",

    ".asc",

    ".las",

    ".jpg",

    ".jp2",

    ".jpeg",

    ".xyz",

    ]

    schemaList = [

    "o1_agriculture_aquaculture",

    "o2_boundary",

    "o3_ict",

    "o4_demographic",

    "o5_education",

    "o6_energy",

    "o7_environment",

    "o8_health",

    "o9_hydrology",

    "o10_landuse",

    "o11_marine",

    "o12_poi",

    "o13_raster",

    "o14_security_safety",

    "o15_survey",

    "o16_topographic",

    "o17_transportation",

    "o18_utilities",

    "oo4_gis_data_catalogue",

    "oo5_urban_models",

    "OO6_original_data",

    "OO7_archived_data",

    ]

  

    def __init__(self):

        super().__init__()

  

    def createInstance(self):

        return type(self)()

  

    def initAlgorithm(self, config=None):

        try:

            # userP = None

            try:

                userDBConns = self.getConnections()  # ,databaseList

                firstConn = list(userDBConns.keys())[0]

                firstVals = userDBConns.get(firstConn)

                userP = firstVals[1]

                passP = firstVals[2]

                databaseList = list(userDBConns.keys())

            except:

                QgsMessageLog.logMessage(

                    message=str(

                        "NO POSTGIS CONNECTIONS SETUP YET, please add one and re-run tool"

                    ),

                    tag="BH- Table Owner",

                    level=1,

                )

            hostP = self.hostL[0]

            try:

                databaseListLive = self.getDatabases(hostP, userP, passP)

            except:

                QgsMessageLog.logMessage(

                    message=str("Cannot get dynamic list of database"),

                    tag="BH- Table Owner",

                    level=1,

                )

  

            try:

                hostP = "sql-gis01"

                rolesL, usersL = self.getRoles(hostP, userP, passP)

                # model_feedback.pushInfo(str(rolesL))

            except:

                QgsMessageLog.logMessage(

                    message=str(

                        "Cannot get roles. User: {} Password: {}".format(userP, passP)

                    ),

                    tag="BH- Execute Privileges",

                    level=1,

                )

                rolesL = ["bhgisreader", "bhgiseditor"]

  

            if userP != None:

                self.addParameter(

                    QgsProcessingParameterEnum(

                        self.serverChoice,

                        self.tr("Choose Server to apply permissions to"),

                        options=self.hostL,

                        defaultValue=0,

                        optional=True,

                    )

                )

  

                self.addParameter(

                    QgsProcessingParameterEnum(

                        self.DatabaseChoice,

                        self.tr('Choose from "Live" Databases'),

                        options=databaseListLive,

                        defaultValue=None,

                        optional=True,

                    )

                )

  

                # self.addParameter(QgsProcessingParameterEnum(self.databaseConnection,tr('Choose from your PostGIS Database connections'), options=databaseList, defaultValue=None, optional=True))

  

                self.addParameter(

                    QgsProcessingParameterString(

                        self.filterString,

                        "Specify username by string (separate by comma)",

                        defaultValue="",

                        optional=True,

                    )

                )

  

                self.addParameter(

                    QgsProcessingParameterEnum(

                        self.userChoiceM,

                        "Or Choose from list of usernames",

                        options=usersL,

                        allowMultiple=True,

                        defaultValue=None,

                        optional=True,

                    )

                )

  

                self.addParameter(

                    QgsProcessingParameterFolderDestination(

                        self.exportFolder,

                        "Specify export folder",

                        defaultValue=r"C:\Temp\QGIS_Tool_Results",

                        optional=True,

                    )

                )

  

            else:

                databaseList = []

                self.addParameter(

                    QgsProcessingParameterEnum(

                        self.DatabaseChoice,

                        self.tr(

                            "ERROR: PLEASE SETUP POSTGIS CONNECTION(S) FIRST (or delete and recreate connection) - then re-run tool. You also need Cisco VPN connection to BH Network)"

                        ),

                        options=databaseList,

                        defaultValue=None,

                        optional=False,

                    )

                )

  

        except:

            QgsMessageLog.logMessage(

                message=str(

                    "Cannot connect to database to get table information. Is wifi on and cisco connected to BH network? {}".format(

                        self.excep()

                    )

                ),

                tag="BH- Table Owner",

                level=1,

            )

  

    def processAlgorithm(self, parameters, context, model_feedback):

        errors = []

        results = {}

        outputs = {}

  

        try:

            serverP = self.parameterAsString(parameters, self.serverChoice, context)

            databaseChoiceP = self.parameterAsString(

                parameters, self.DatabaseChoice, context

            )

            databaseConnP = self.parameterAsString(

                parameters, self.databaseConnection, context

            )

  

            stringP = self.parameterAsString(parameters, self.filterString, context)

            userChoiceMCh = self.parameterAsEnums(parameters, self.userChoiceM, context)

            exportFolderP = self.parameterAsString(

                parameters, self.exportFolder, context

            )

  

            stringL = []

            if "," in stringP:

                stringL = stringP.split(",")

                stringL = [s.strip() for s in stringL]

            elif "\t" in stringP:

                stringL = stringP.split("\t")

                stringL = [s.strip().replace("\n", "") for s in stringL if len(s) > 0]

            elif "\n" in stringP and "\t" not in stringP:

                stringL = stringP.split("\n")

                stringL = [s.strip() for s in stringL if len(s) > 0]

            else:

                stringL.append(stringP)

  

            try:

                if not os.path.exists(exportFolderP):

                    os.makedirs(exportFolderP)

            except:

                model_feedback.pushInfo(self.excep())

  

            dateTime = (str(datetime.now())[0:19]).replace(":", "").replace(" ", "_")

            resultFile = os.path.join(

                exportFolderP, "Table_Ownership_{}.txt".format(dateTime)

            )

  

            try:

                outfile = open(resultFile, "w")

                outfile.write("Server\tDatabase\tSchema\tTable\tOwner")

  

            except:

                model_feedback.pushInfo(self.excep())

  

            userP = "bhdba"  # DBA is preferred as can see all tables

            passP = "bhdbaPlat72"

  

            hostP = hostL[int(serverP)]

            try:

                hostP = hostL[int(serverP)]

                databaseListLive = self.getDatabases(hostP, userP, passP)

            except:

                model_feedback.pushInfo(

                    "Cannot get dynamic list of database: {}".format(self.excep())

                )

            portP = 5432

            databaseCh = databaseListLive[int(databaseChoiceP)]

  

            # try:

            #    outfile = open (resultFile, "w")

            #    outfile.write("Server\tDatabase\tSchema\tTable\tColumns\tColumns_Types")

            # except:

            #    model_feedback.pushInfo(excep())

  

            try:

                rolesL, usersL = self.getRoles(hostP, userP, passP)

                # model_feedback.pushInfo(str(rolesL))

            except:

                model_feedback.pushInfo(

                    "\Cannot get list of roles, defaulting to basic list"

                )

  

            usersCh = []

            if len(userChoiceMCh) != 0:

                for fl in userChoiceMCh:

                    userX = usersL[int(fl)]

                    usersCh.append(userX)

  

            usersRoles = usersCh

  

            model_feedback.pushInfo(

                str(

                    "Chosen default live Database: '{}'. {} {} {}".format(

                        databaseCh, hostP, portP, userP

                    )

                )

            )

  

            tableList, tableDict = self.getDatabaseTables(

                hostP, databaseCh, userP, passP

            )  # gets table properties by scanning the database

  

            if len(stringL) > 0:

                # tableList = [t for t,dat in tableDict.items() if len([s for s in stringL if s.lower() in dat[1].lower()]) > 0]

                tableDict = {

                    t: dat

                    for (t, dat) in tableDict.items()

                    if len([s for s in stringL if s.lower() in dat[1].lower()]) > 0

                }

  

            if len(usersRoles) > 0:

                tableDict = {

                    t: dat

                    for (t, dat) in tableDict.items()

                    if len([s for s in usersRoles if s.lower() in dat[1].lower()]) > 0

                }

            # model_feedback.pushInfo("\nMatched tables:\n{}".format('\n'.join(tableList)))

            test = 0

  

            if len(tableList) > 0 and test == 0:

                # tableDict = {}

                # id = 0

                ## Renames tables in database

                uniqueOwnerL = list(set([dat[1] for t, dat in tableDict.items()]))

                for owner in uniqueOwnerL:

                    model_feedback.pushInfo("\nTables under owner: {}".format(owner))

                    ownerTabL = [t for t, dat in tableDict.items() if dat[1] == owner]

                    ownerTabL = sorted(ownerTabL)

                    for tab in ownerTabL:

                        schema = [dat[0] for t, dat in tableDict.items() if t == tab][0]

                        outfile.write(

                            "\n{}\t{}\t{}\t{}\t{}".format(

                                hostP, databaseCh, schema, tab, owner

                            )

                        )

  

                    model_feedback.pushInfo(

                        "Table Count: {}\n{}".format(

                            len(ownerTabL), "   \n".join(ownerTabL)

                        )

                    )

  

            model_feedback.pushInfo("\n\n")

  

            file = 0

            if file == 1:

                try:

                    outfile.close()

                    model_feedback.pushInfo(

                        "\nSee layer list results (drag and drop into Excel): {}\n".format(

                            resultFile

                        )

                    )

                    excelApp = None

                    excelroot = r"C:\Program Files\Microsoft Office"

                    for root, dirnames, filenames in os.walk(excelroot):

                        if len(filenames) > 0:

                            for file1 in filenames:

                                if "EXCEL.EXE" == file1.upper():

                                    excelApp = os.path.join(root, file1)

  

                    import subprocess

  

                    if excelApp == None:

                        os.startfile(os.path.dirname(resultFile))

                    else:

                        # model_feedback.pushInfo(excelApp)

                        subprocess.Popen("%s %s" % (excelApp, resultFile))

  

                except:

                    model_feedback.pushInfo(

                        "\nFile LOCKED! - you have the output txt file open '{}'. Close it and run again\n".format(

                            resultFile

                        )

                    )

                    model_feedback.pushInfo(self.excep())

  

            #    if len(exportFolderP) > 1:

            #        try:

            #            destPack = os.path.join(exportFolderP,"SpatialExtents.gpkg")

            #            try:

            #                pass

            #                #shutil.copytree(geoPack,destPack)

            #            except:

            #                excep()

            #

            #        except:

            #            model_feedback.pushInfo(excep())

        except:

            model_feedback.pushInfo(self.excep())

  

        return results

  

    def name(self):

        return "DBA23. Get Table Owners"

  

    def displayName(self):

        return "DBA23. Get Table Owners"

  

    def group(self):

        return "DBA"

  

    def groupId(self):

        return "DBA"

  

    def shortHelpString(self):

        mes = """

        WARNING: If tool is blank, please setup at least 1 PostGIS connection, or delete and recreate the connection

        This tool allows DBAs to dget the table and table owner in the database

  

        """

        return tr(mes)

    def getConnections(self):

        userDBConns = {}

        try:

            try:

                from PyQt5.QtCore import QSettings

            except:

                try:

                    from PyQt4.QtCore import QSettings

                except:

                    QgsMessageLog.logMessage(

                        message=str(

                            "Cannot get PostGIS connections. {}".format(self.excep()),

                            tag="BH- Table Owner",

                            level=1,

                        )

                    )

            userDBConnections = []

            s = QSettings()

  

            s.beginGroup("PostgreSQL/connections")

  

            hostL = [s.value(k) for k in s.allKeys() if "host" in k]

            portL = [s.value(k) for k in s.allKeys() if "port" in k]

            dbL = [s.value(k) for k in s.allKeys() if "database" in k]

            connL = [k.split("/")[0] for k in s.allKeys() if "database" in k]

            userL = [

                s.value(k.replace("database", "username"))

                for k in s.allKeys()

                if "database" in k

            ]

            passL = [

                s.value(k.replace("database", "password"))

                for k in s.allKeys()

                if "database" in k

            ]

  

            # QgsMessageLog.logMessage(message=str("{}".format(hostL)), tag="BH- Table Owner", level=1)

            # QgsMessageLog.logMessage(message=str("{}".format(dbL)), tag="BH- Table Owner", level=1)

  

            for i in range(0, len(hostL)):

                connName = connL[i]

                hostP = hostL[i]

                portP = portL[i]

                database = dbL[i]

                userP = userL[i]

                passP = passL[i]

                userDBConns.update({connName: [database, userP, passP, hostP, portP]})

                # QgsMessageLog.logMessage(message="Conn: {}. Host: {}. Port: {} DB: {}. User: {}. Pass: {}.".format(connName,hostP,portP,database,userP,passP), tag="BH- Clean Column Names", level=1)

  

            s.endGroup()

            # databaseList = [connName + " ["+ details[0]+"]" for connName,details in userDBConns.items()]

            return userDBConns  # ,databaseList

        except:

            QgsMessageLog.logMessage(

                message=str("Cannot get PostGIS connections. {}".format(self.excep())),

                tag="BH- Table Owner",

                level=1,

            )

            databaseList = [tr("ksa"), tr("uk"), tr("chl"), tr("rou")]

    def getDatabaseTables(self, hostP, databaseP, userP, passP):

        try:

            tableList = []

            rasterList = []

            tableDict = {}

            tableSize = {}

            rasterDict = {}

            rasterFiles = {}

  

            connection = psycopg2.connect(

                user=userP, password=passP, host=hostP, port="5432", database=databaseP

            )

            cursor = connection.cursor()

  

            query01 = "SELECT t.table_schema,c.relname, u.usename from information_schema.tables t join pg_catalog.pg_class c on (t.table_name = c.relname) join pg_catalog.pg_user u on (c.relowner = u.usesysid) WHERE t.table_schema NOT IN ('tiger','topology','public')"

            query02 = (

                "SELECT r_table_schema,r_table_name,r_raster_column FROM raster_columns"

            )

            query03 = "SELECT tablename,fullpath,class FROM oo4_gis_data_catalogue.published_rasters"

  

            try:

                cursor.execute(query01)

                records = cursor.fetchall()

                ##QgsMessageLog.logMessage(message=str(records), tag='BH- Table Owner', level=1)

                for rec in records:

                    if rec[0] == None or rec[0] == "":

                        val1 = ""

                    else:

                        val1 = rec[0]

                    if rec[1] == None:

                        val2 = ""

                    else:

                        val2 = rec[1]

  

                    schema = val1

                    table = val2

                    # records = rec[2]

                    user = rec[2]

                    if table not in tableList:

                        tableList.append(table)

  

                    tableDict.update({table: [schema, user]})

                    # tableSize.update({table : str(records)})

            except:

                print(self.excep())

  

            try:

                cursor.execute(query02)

                records = cursor.fetchall()

                # QgsMessageLog.logMessage(message=str(records), tag='BH- Table Owner', level=1)

                for rec in records:

                    rasterDict.update({rec[1]: rec[0]})

                    rasterList.append(rec[1])

            except:

                self.excep()

            try:

                cursor.execute(query03)

                records = cursor.fetchall()

                # QgsMessageLog.logMessage(message=str(records), tag='BH- Table Owner', level=1)

                for rec in records:  # table : Fullpath,class

                    rasterFiles.update({rec[0]: [rec[1], rec[2]]})

            except:

                self.excep()

            cursor.close()

            connection.close()

  

            tableList.sort()

            return tableList, tableDict

  

        except (Exception, psycopg2.Error) as error:

            mes = "Error while fetching data from PostgreSQL: " + str(error) + self.excep()

            QgsMessageLog.logMessage(message=str(mes), tag="BH- Table Owner", level=1)

    def getRoles(self, hostP, userP, passP):

        try:

            connection = psycopg2.connect(

                user=userP, password=passP, host=hostP, port="5432", database="postgres"

            )

            cursor = connection.cursor()

            query01 = "SELECT rolname FROM pg_roles WHERE rolcanlogin = False AND rolname NOT LIKE 'pg_%';"

            query02 = "SELECT rolname FROM pg_roles WHERE rolcanlogin = True AND rolname NOT LIKE 'postgres'"

            rolesL = []

            usersL = []

            try:

                cursor.execute(query01)

                records = cursor.fetchall()

                # QgsMessageLog.logMessage(message=str(records), tag='BH- Execute Privileges', level=1)

                for rec in records:

                    rolesL.append(rec[0])

            except (Exception, psycopg2.Error) as error:

                mes = "Error while fetching roles PostgreSQL: " + str(error) + self.excep()

                QgsMessageLog.logMessage(

                    message=str(mes), tag="BH- Execute Privileges", level=1

                )

            try:

                cursor.execute(query02)

                records = cursor.fetchall()

                # QgsMessageLog.logMessage(message=str(records), tag='BH- Execute Privileges', level=1)

                for rec in records:

                    usersL.append(rec[0])

            except (Exception, psycopg2.Error) as error:

                mes = "Error while fetching roles PostgreSQL: " + str(error) + self.excep()

                QgsMessageLog.logMessage(

                    message=str(mes), tag="BH- Execute Privileges", level=1

                )

  

            cursor.close()

            connection.close()

            rolesL.sort()

            usersL.sort()

            # QgsMessageLog.logMessage(message=str(rolesL), tag='BH- Execute Privileges', level=1)

            return rolesL, usersL

  

        except (Exception, psycopg2.Error) as error:

            mes = "Error while fetching roles PostgreSQL: " + str(error) + self.excep()

            QgsMessageLog.logMessage(

                message=str(mes), tag="BH- Execute Privileges", level=1

            )

    def getDatabases(self, hostP, userP, passP):

        databaseL = []

        try:

            connection = psycopg2.connect(

                user=userP, password=passP, host=hostP, port="5432", database="postgres"

            )

            connection.autocommit = True

            cursor = connection.cursor()

  

            query00 = "SELECT datname FROM pg_database WHERE datname NOT LIKE 'template%' AND datname NOT LIKE 'post%' AND datname NOT LIKE 'bh_gis%';"

            cursor.execute(query00)

            records = cursor.fetchall()

            for rec in records:

                databaseL.append(rec[0])

                # Dict.update({rec[1]:rec[0]})

            cursor.close()

            connection.close()

            return databaseL

  

        except (Exception, psycopg2.Error) as error:

            mes = "Error getting list of databases: " + str(error) + self.excep()

            QgsMessageLog.logMessage(message=str(mes), tag="BH- Table Owner", level=1)

  

    def tr(self, string):

        return QCoreApplication.translate(string)

  

    def excep(self):

        tb = sys.exc_info()[2]

        tbinfo = traceback.format_tb(tb)[0]

        errorM = str(sys.exc_info()[1]).split("\n")[0]

        if "," in errorM:

            errorM = errorM.split(",")[0] + ". " + errorM.split(",")[1]

        lineN = str(tbinfo).split(", ")[1]

        mess = "Error  [" + lineN + "] " + errorM

        return mess

  

    def createInstance(self):

        return CleanColumns()

```

