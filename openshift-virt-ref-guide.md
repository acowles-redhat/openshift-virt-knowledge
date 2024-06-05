OpenShift Virtualization
ReferenceImplementationGuide
HybridPlatformsBusinessUnit
version:1.0.
May 2024
LegalNotice...............................................................................................................................................
Copyright© 2024 RedHat,Inc.
ThetextofandillustrationsinthisdocumentarelicensedbyRedHatunderaCreative
CommonsAttribution–ShareAlike3.0Unportedlicense("CC-BY-SA").Anexplanationof
CC-BY-SAisavailableathttp://creativecommons.org/licenses/by-sa/3.0/.Inaccordancewith
CC-BY-SA,ifyoudistributethisdocumentoranadaptationofit,youmustprovidetheURLfor
theoriginalversion.

RedHat,asthelicensorofthisdocument,waivestherighttoenforce,andagreesnottoassert,
Section4dofCC-BY-SAtothefullestextentpermittedbyapplicablelaw.

RedHat,RedHatEnterpriseLinux,theShadowmanlogo,theRedHatlogo,JBoss,OpenShift,
Fedora,theInfinitylogo,andRHCEaretrademarksofRedHat,Inc.,registeredintheUnited
States
andothercountries.

Linux®istheregisteredtrademarkofLinusTorvaldsintheUnitedStatesandothercountries.

Java®isaregisteredtrademarkofOracleand/oritsaffiliates.

XFS®isatrademarkofSiliconGraphicsInternationalCorp.oritssubsidiariesintheUnited
States
and/orothercountries.

MySQL®isaregisteredtrademarkofMySQLABintheUnitedStates,theEuropeanUnionand
othercountries.

Node.js®isanofficialtrademarkofJoyent.RedHatisnotformallyrelatedtoorendorsedbythe
officialJoyentNode.jsopensourceorcommercialproject.

TheOpenStack®WordMarkandOpenStacklogoareeitherregisteredtrademarks/service
marks
ortrademarks/servicemarksoftheOpenStackFoundation,intheUnitedStatesandother
countriesandareusedwiththeOpenStackFoundation'spermission.Wearenotaffiliatedwith,
endorsedorsponsoredbytheOpenStackFoundation,ortheOpenStackcommunity.

Allothertrademarksarethepropertyoftheirrespectiveowners.

TableofContents....................................................................................................................................
Table ofContents
LegalNotice...............................................................................................................................................
TableofContents....................................................................................................................................
AboutthisDocument.............................................................................................................................
Purposeofthisdocument........................................................................................................
TargetUseCases....................................................................................................................
ReplatformingfromLegacyVirtualizationPlatforms..........................................................
ModernizeTodayForInnovationTomorrow.......................................................................
ConsistencyOfManagement.............................................................................................
IncreasedOperationalEfficiency.......................................................................................
TechnologyOverview.....................................................................................................................
Recommendeddesignandarchitecture...................................................................................
PhysicalClusterDesign.......................................................................................................................
Micro................................................................................................................................
Small................................................................................................................................
Medium...........................................................................................................................................
Large................................................................................................................................
applications.......................................................................................................................... Featureandfunctionalityimplementationtosupportvirtualmachinesand
Clusterdeploymentconfiguration....................................................................................
InstallerProvisionedInfrastructure............................................................................
RedHatEnterpriseLinuxCoreOS(RHCOS)............................................................
BondedNICsformanagementandSDN...................................................................
Post-installconfiguration..................................................................................................
OpenShiftVirtualizationOperatorconfiguration.........................................................
Machineandnodeconfiguration.........................................................................................
Additionaldedicatednetworkinterfacesfortraffictypes............................................
Kubeletconfiguration...........................................................................................................
OpenShiftDataFoundationconfiguration..................................................................
Multi-networkpolicyactivation...........................................................................................
AdditionalOperatorconfiguration..............................................................................
Additionalfeatures..............................................................................................................
Compute..........................................................................................................................
GPU/vGPUaccess................................................................................................................
Hardware/PCIPassthrough.................................................................................................
Network............................................................................................................................
Software-DefinedNetworking(SDN)...............................................................................
AdditionalSDNOptions.................................................................................................
NetworkingArchitecture...............................................................................................
Storage............................................................................................................................
CSIProvider...........................................................................................................................
Importantconceptsandadditionalknowledge......................................................................
BusinessContinuity...........................................................................................................................
HighAvailability..................................................................................................................................
DisasterRecovery...............................................................................................................................
Scalability............................................................................................................................................
SubscriptionInformation...................................................................................................................
SolutionSummary.....................................................................................................................
AdditionalLinksandReferences.............................................................................................
Documentation:..................................................................................................................................
Videos:.................................................................................................................................................
AppendixA.Detailedresourcecalculations...........................................................................
CPUcapacitycalculation..................................................................................................................
Memorycapacitycalculation............................................................................................................
ODFstoragecapacitycalculation...................................................................................................
AppendixB.Changelog...........................................................................................................
AboutthisDocument.............................................................................................................................
Purposeofthisdocument........................................................................................................
Thisdocumentprovidesimplementationguidelinesandasamplereferencearchitecturefor
deployingRedHatOpenShiftasaplatformforvirtualizationworkloadsusingOpenShift
Virtualization.Thedesignguidelinesandarchitecturedescribedprovideatargetplatformto
hostworkloadsthatmaycurrentlyresideononeofthefollowingplatforms:

● VMwareCloudFoundation
● VMwarevSphereFoundation
● RedHatVirtualization
● OpenStack
Thisdocumentisasupplementtoproductdocumentationprovidingopinionatedsuggestions
andguidanceforconfiguringOpenShiftforhostingvirtualmachines.Itsuggestsseveralcluster
architecturesforvaryingcapacityneeds,providesanoverviewofthetechnologyinuse,
providesdesignconsiderations,andprovidesprescriptiveguidanceforimplementation.The
guidelinesinthisdocumentdonotdefinearequiredconfigurationforsupport,norarethey
exhaustive.Beforeimplementing,considertheneedsandrequirementsofyourvirtualized
applicationsandchoosetheconfigurationthatbestmeetsthoseneeds.

TargetUseCases....................................................................................................................
OpenShiftVirtualizationhelpscustomersoptimizetheirdatacenterfootprintfortoday'sand
tomorrow’sapplications.ItprovidesmoderninfrastructureforenterpriseVMsthatincreases
efficiencythroughautomation,manageability,andscalability.OpenShiftVirtualizationdelivers
theperformance,scale,andstabilityofKernel-basedvirtualmachines(KVM),deployedand
managedaspartofamodernapplicationplatform.

OpenShiftVirtualizationenablesbusinessestoaccelerateinfrastructuremodernizationby
bringingmodernvirtualizationinfrastructure,viaOpenShift,tosupportexistingvirtual
machines.

Customerswillseevalueinseveralways:

ReplatformingfromLegacyVirtualizationPlatforms..........................................................
OpenShiftisatrusted,comprehensive,andconsistentplatformdesignedtoscalewiththe
needsoftoday’svirtualizationworkloads.AscustomersmovetoOpenShiftVirtualizationfroma
legacyplatform,theywillseeimmediatebenefitsasthemanagementofvirtualmachinesshifts
toacloud-nativeparadigmviaOpenShift’sKubernetesfoundation.

ModernizeTodayForInnovationTomorrow.......................................................................
WithOpenShiftVirtualization,organizationscanmodernizetheirvirtualizationplatformand
benefitfrommodernmanagementprinciplesintegratedintoOpenShift.ByaddingexistingVMs
tomixedapplications,customerscanruntheirvirtualizationmachinesandcontainerssideby
side.

ConsistencyOfManagement.............................................................................................
FromtheOpenShiftconsole,customerscanmanageVMs,containers,andserverlessinone
place.ConsistentmanagementofworkloadswilldrivebetteralignmentbetweenVMadminsand
platformengineeringteams.

IncreasedOperationalEfficiency.......................................................................................
Theself-serviceoptionsinOpenShifthelpteamsimprovetheirtimetoproduction.Specifically,
customerscanstreamlinedeploymentsoftheirapplicationanddevelopmentstacksby
integratingVMswithdevelopmentpipelinesviatheOpenShiftAPIandintegratingtheCI/CD
pipelinesdirectlyintoOpenShiftdeployments.

TechnologyOverview.....................................................................................................................
OpenShiftVirtualizationusesKVM,theLinuxkernelhypervisor,tobringvirtualmachinesas
nativeobjectstoOpenShift,withthefeaturesandcapabilitiesofOpenShiftsupportingcluster
managementandscalability,alongwithvirtualmachineconsumption.KVMisacorecomponent
oftheRedHatEnterpriseLinuxkernelandhasbeenusedinproductionfor15+yearsinother
hypervisorproducts,suchasRedHatVirtualization,RedHatOpenStackPlatform,andRedHat
EnterpriseLinux.TheKVMusedbyOpenShiftisthesameKVM,QEMU,andlibvirtusedby
thoseplatforms;OpenShiftisarobust,stable,andscalabletype-1hypervisorforvirtualization
workloads.

Virtualmachinesaredeployedandmanagedusingthesameschedulerthatisusedfor
non-virtualmachineworkloads.Nativefeatureslikehostaffinity,resourceawareness,load
balancing,andhighavailabilityareallinheritedfromOpenShiftfeaturesfornon-virtualmachine
workloads.

Virtualmachinesconnectedtotheclustersoftware-definednetwork(SDN)areaccessibleusing
standardKubernetesmethods,includingServicesandRoutes,whilealsobeingsubjectto
controllingtrafficusingnetworkpolicyandingress/egressconfiguration.Thisprovidesrobust
methodsforenablingaccesstovirtualmachinesfrominsideandoutsidethecluster,alongwith
platform-levelsecuritycontrolsformanagingaccesstoVM-hostedapplications.

OpenShiftVirtualizationhasanumberofdeploymentoptionsavailable,includingself-managed
baremetalhostson-premises,orinthecloudonAWSusingeithermanagedROSAclustersor
self-managedclusters.

Referencedesign and architecture
Werecommendamodulararchitecturewithastructured,standardizedapproachfordesigning
andimplementingRedHatOpenShiftVirtualization.Thisframeworkguidescustomers,sales,
andservicesteamsthroughastandardizedapproachthatfacilitatesscalability,security,and
compliancewithindustrystandards.Byleveragingthesedesignguidelines,organizationscan
acceleratetheirdigitaltransformationinitiatives,improvesysteminteroperability,andachieve
moreefficientandeffectiveIToperations.Itservesasablueprint,offeringbestpractices,
patterns,andguidelinesthataddresscurrentandfuturetechnologicalchallenges,enabling
businessestoinnovateandadaptinarapidlychangingdigitallandscape.

PhysicalClusterDesign.......................................................................................................................
Intheabsenceofknownsizingforcomputeandstorageresources,RedHatrecommendsusing
amodularapproachtonodesizingbasedontheexpectedinitialcapacityrequirements,withthe
abilitytoscaleupto100+hypervisornodesinasingleOpenShiftdeployment.OpenShift
Virtualizationsupportsmanydifferentnodeandclusterconfigurations.Intheabsenceofknown
sizingrequirements,thesizingbelowrepresentsastartingpointforanOpenShiftdeployment;
however,nodesizesandconfigurationsshouldbetailoredtomeetyourrequirementsas
needed.

Hereisagraphicalrepresentationofthedifferentnodetypesaspartofthissolution:

Micro................................................................................................................................
Thisclustersizeiscomprisedof:
● Athree(3)nodeOpenShiftcluster
○ Controlplanerunningonthree(3)nodes
○ Storageandcomputerunningonallthree(3)nodeswithcompute
○ Examplenodesize:
■ CPU: 64 cores
■ Memory:512GiB
■ OSDisks:2x512GB(RAID1)
■ ClusterStorage(ODF):2x8TBNVMe/SSDdisks
■ Network:6x25GbNICs
○ Theresultingclusterhas 342 vCPU(with4:1overcommit),826GiBmemory,and
10.4TBstorageavailableforworkloads.
■ 34%ofcomputeandmemorycapacityisreservedforHA
■ 10%ofcomputecapacityisreservedforspare/burst
■ Thesuggestedstoragecapacityabovereflects65%ofthecapacityused

Thehigh-leveldesignforthemicroformfactorclusterwouldlooklikethefollowing:

Small................................................................................................................................
Thisclustersizeiscomprisedof:
● Ten(10)nodeOpenShiftcluster
○ Controlplane+computerunningonthree(3)nodes
○ Storagerunningonthree(3)nodeswithcompute
○ Additionalfour(4)nodesof“just”compute

○ Examplenodesize:
■ CPU: 64 cores
■ Memory:512GiB
■ OSDisks:2x512GB(RAID1)
■ ClusterStorage(ODF):8x8TBNVMe/SSDdisks
■ Network:6x25GbNICs
○ Theresultingclusterhas 1532 vCPU,3496GiBmemory,and41.6TBstorage
availableforworkloads.
■ 20%ofcomputeandmemorycapacityisreservedforHA
■ 10%ofcomputecapacityisreservedforspare/burst
■ Thesuggestedstoragecapacityabovereflects65%ofthecapacityused
Theresultingclusterhasthreenodetypes(compute,compute+storage,compute+control
plane)whichcontributecapacityforhostingvirtualmachinesinadditiontotheirsharedpurpose
(ODForcontrolplane).

Hereisahigh-leveldesignforthesmallformfactorcluster:

Medium...........................................................................................................................................
Thisclustersizeiscomprisedof:
● Fifteen(15)nodeOpenShiftcluster
○ Controlplane+computerunningonthree(3)nodes
○ Storagerunningonthree(3)nodeswithcompute
○ Additionalnine(9)nodesof“just”compute
○ Examplenodesize:
■ CPU: 64 cores
■ Memory:512GiB
■ OSDisks:2x512GB(RAID1)
■ ClusterStorage(ODF):8x8TBNVMe/SSDdisks
■ Network:6x25GbNICs
○ Theresultingclusterhas 2345 vCPU,5177GiBmemory,and41.6TBstorage
availableforworkloads.
■ 10%ofcomputeandmemorycapacityisreservedforHA
■ 10%ofcomputecapacityisreservedforspare/burst
■ Thesuggestedstoragecapacityabovereflects65%ofthecapacityused

Theresultingclusterhasthreenodetypes(compute,compute+storage,compute+control
plane)whichcontributecapacityforhostingvirtualmachinesinadditiontotheirsharedpurpose
(ODForcontrolplane).

Hereisahigh-leveldesignforthemediumformfactorcluster:

Large................................................................................................................................
● Scalethemediumdesignfurther,takingintoconsiderationthatwemayconsider
non-convergedstoragebasedonusecasesandclusterarchitectures:
○ Controlplanerunningonthree(3)nodes
○ Storagerunningonaminimumof 3 nodeswithcompute
○ Computerunningonallnodes
○ Scalestoragenodesinincrementsoffour,whichalignswithODFentitlementsof
256TBrawstorage
○ Scalecomputenodesinincrementsofone(1)
WedorecommendthatwhendeployingOpenShiftVirtualization,failuredomains
are alsodefined at thephysical architecture layer whenpossibletoprovide
betterresiliency.TopologylabelscanbeappliedtonodesandusedbyVMsand
containerizedapplicationstospreadacrossfailuredomains.Afewexamplesof
spreadingacrossfailuredomainsare:
● Useamulti-racktopology(atleast3),distributingtherolesacrossthe
multipleracks
● Eachrackisafailuredomain
● DeployaCLOSLeaf-Spinearchitecture,whereeachrackiscontained
withinaLeaf
Formoreinformation,checkthefollowingreferences:
https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spre
ad-constraints/#node-labels
https://kubernetes.io/docs/reference/labels-annotations-taints/#topolo
gykubernetesioregion
Referenceimplementation
DeployingandconfiguringOpenShiftfeaturestosupport
virtualmachinesandapplications
Usingtheaboveclustergeometryasastartingpoint,theOpenShiftcluster(s)needtohave
featuresandfunctionsconfiguredtoprovidecapabilitiesforsupportingthedeploymentand
managementofvirtualmachines,alongwithvalue-addcapabilitiesforvirtualizedapplications.
ThissectionwilldescribetherecommendedOpenShiftconfigurationforhostingvirtual
machines.

Clusterdeploymentconfiguration....................................................................................
InstallerProvisionedInfrastructure............................................................................
Thisarchitectureusestheinstallerprovisionedinfrastructure(IPI)methodfordeployingthe
cluster.Theresultingclusterwillhavethebaremetalcloudproviderconfiguredandwillbeable
tomakeuseofitforaddingandmanagingnodes.FormoreinformationonIPI-basedbaremetal
installations,pleaseseethedocumentationhere.

Forthesuggestedclusterdesignsinthisreferencearchitecture,thecontrolplaneshouldbe
schedulable.Wheninstantiatingthecluster,deployathreenode“compact”cluster,whichwill

markthecontrolplanenodesasschedulablebydefault.Afterdeployment,joinadditional
computeandstoragenodesasneeded,usingdistinctMachineSetsforeachrole,e.g.storage
andcompute.

RedHatEnterpriseLinuxCoreOS(RHCOS)............................................................
OpenShiftVirtualizationrequiresRedHatEnterpriseLinuxCoreOS(RHCOS)computenodes.
EventhoughitispossibletodeployanduseRedHatEnterpriseLinux(RHEL)computenodes,
theyareincompatiblewithOpenShiftVirtualization.RHCOSisbasedonRHELanddesignedto
runcontainerswithanintegratedCRI-Oruntimeanddedicatedcontainertools.Theoperating
systemusesrpm-ostreetofacilitatebeingmanagedthroughtheMachineConfigOperatorto
deployoperatingsystemupdatesandapplyconfigurationdeclaratively.

TocustomizeRHCOS,forexample,toadddriversorotherthird-partyOS-leveltools,the
RHCOSimagelayeringfeaturemustbeusedtomodifytheoperatingsystemandaddthe
appropriatepackages.

FormoreinformationonRedHatEnterpriseLinuxCoreOS,seethedocumentation.

BondedNICsformanagementandSDN...................................................................
Theinitialbondinterface,consistingoftwoadaptersbondedtogetherwithanIPaddressonthe
machinenetworkspecifiedandconfiguredatinstalltime,isusedfortheSDN,management
trafficbetweenthenodeandthecontrolplane(andadministratoraccess),andlivemigration
traffic.Duringinstallation,usethehostnetworkinterfaceconfigurationoptionstoconfigurethe
bondandsettheIPaddressneeded.

Post-installconfiguration..................................................................................................
AftertheOpenShiftclusterhasbeendeployed,withadditionalnodesjoinedforcomputeand
storageroles,additionalclusterconfigurationisneededtoadjustandoptimizetheconfiguration
forvirtualmachines.

OpenShiftVirtualizationOperatorconfiguration.........................................................
OpenShiftVirtualizationisdeployedandconfiguredintheclusterusingOperatorHub.Once
installed,thereareseveralconfigurationoptionstoset.

FollowthedocumentationfordeployingtheOperatorandcreatinganinstanceofthe
HyperConvergedresource.Thebelowbulletsrepresentcommondecisionsandconfiguration
parametersthatarenotdefault.

● Decidewhethertodownloadthedefault,RedHatprovided,operatingsystemimages.
TheOperatorwillautomaticallyretrieveandmakeavailablecloudimagesforRHEL,
Fedora,andCentOSwhenadefaultstorageclassisconfigured.Ifyoudonotintendto
usetheseimages,itisrecommendedtodisableautomaticallyretrievingthem.
● ConfigurethedefaultCPUmodeltothelowestcommondenominatorCPUmodelinthe
cluster.ThesevaluesaredeterminedbytheCPUvendorandthehighest,orlowest,CPU
modelavailableintheOpenShiftcluster.ForAMDCPUs,avalueofEPYCis
recommended,forIntelCPUsavalueequivalenttothegenerationname,e.g.
Cascadelake-Server,isrecommended.
● IfyoudonotwanttousethedefaultstorageclassforstoringVMstaterelatedto,among
otherthings,vTPM,setthedesiredstorageclasswhendeployingOpenShift
Virtualization.ThisstorageclassmustsupportRWXaccessmode.
Machineandnodeconfiguration.........................................................................................
Thesetasksrepresentconfigurationsappliedtotheclusternodesaftertheclusterisdeployed.

● Configuretimesynchronizationtoavoidclockdriftandissuesrelatedto
inaccurate/inconsistenttime,suchascertificatesbeingincorrectlymarkedinvalid.
● ConfigurenodehealthchecksandfenceagentsremediationforreducedHArecovery
timeintheeventofnodefailure.
TheNodeHealthCheckOperatorandFenceAgentsRemediationOperatorare
deployedfromOperatorHubinthewebconsole.Onceinstalledaninstanceofthe
FenceAgentsRemediationTemplatemustbecreatedtoprovideBMCconnection
informationforthenodes.Thisisthesameinformationthatwasusedwhendeploying
theclusterandjoiningnodesusingaMachineSet.Finally,createaninstanceofthe
NodeHealthChecktodefineappropriatetimeoutvaluesforyourdesiredVMrecovery
time.RefertothechartinthisKCSfordetailsonhowdifferentvaluesaffecttherecovery
times.
Additionaldedicatednetworkinterfacesfortraffictypes............................................
Thesuggestedhardwareconfigurationusedinthisreferencearchitecturehassixnetwork
adaptersconfiguredinthreepairsoftwoLACPbonds.ThefollowingisasampleNMstate
configurationmakinguseoftwoadaptersonthehosttocreateabondedinterfaceintheLACP
(802.1ad)runmode.

apiVersion: nmstate.io/v
kind: NodeNetworkConfigurationPolicy
metadata:
annotations:
description: a bond for VM traffic and VLANs
name: bonding-policy
spec:
desiredState:
interfaces:
link-aggregation:
mode: 802.3ad
port:
- enp6s0f
- enp6s0f
name: bond
state: up
type: bond
Thebondsareintendedtobeusedforisolatingnetworktrafficfordifferentpurposes.This
providestheadvantageofavoidingnoisyneighborscenariosforsomeinterfacesthatmayhave
alargeimpact,forexampleabackupforavirtualmachineconsumingsignificantnetwork
throughputimpactingODForetcdtrafficonasharedinterface.

● Management,SDN,andlivemigration-thisistheinterfaceconfiguredduring
installation,withanIPonthemachinenetworkCIDR.OpenShiftwillconfiguretheSDN
onthisinterface,anditistheinterfaceexpectedtobeusedfornode-to-controlplane
communication.

Additionally,bydefault,OpenShiftVirtualizationusesthisinterfaceforlivemigration
traffic.Thishasthebenefitofthetrafficbeingencryptedintransit;however,ifSDN
throughputisconstrained,orotherworkloadswouldbeimpacted,thenusingadifferent
network,suchasonesharedwiththeIP-basestorage,forlivemigrationtraffic.
● IP-basedstorage-thisistheinterfacewhichwillbeusedfortwopurposesinthis
referencearchitecture.Usingadditionalisolation,suchasVLANs,onthisinterfaceis
possible,butnotrequired.

First,itistheinterfaceexpectedtobeusedformountingiSCSI,NFS,andRBDvolumes
usedbyvirtualmachinedisks.IfthenodeIPsandthestoragesystemarenotinthesame
broadcastdomain(i.e.L2adjacency)thenadditionalrouterulesmayneedtobecreated
usingNMstatetoensurethattraffictothestoragesystemutilizesthisinterface.
Second,fordeploymentsusingODF,thisistheinterfacetobeusedforODFreplication
traffic.
● Virtualmachinetraffic-thisinterfacewillbeconfiguredwithanOVSbridgetoattach
virtualmachines.ForeachVLANusedbyvirtualmachines,anOVNsecondarynetwork
NetworkAttachmentDefinitioniscreatedusingthelocalnettopologyandaVLAN
identifier.NetworkAttachmentDefinitionsarecreatedinthedefaultnamespacefor
usebyallothernamespaces,orinspecificnamespacestoprovideRBACcontrolsfor
networkconnectivity.

AnexampleconfigurationforVMnetworkconnectivityisbelow,notethatthebond
configurationshouldbeapartofthesameNodeNetworkConfigurationPolicytoensure
theyareconfiguredtogether.
apiVersion: nmstate.io/v
kind: NodeNetworkConfigurationPolicy
metadata:
name: ovs-br1-vlan-trunk
spec:
nodeSelector:
node-role.kubernetes.io/worker: ''
desiredState:
interfaces:

name: ovs-br
description:|-
A dedicated OVS bridge with bond2 as a port
allowing all VLANs and untagged traffic
type: ovs-bridge
state: up
bridge:
options:
stp: true
port:
- name: bond
ovn:
bridge-mappings:
localnet: vlan-
bridge: ovs-br
state: present
localnet: vlan-
bridge: ovs-br
state: present
apiVersion: k8s.cni.cncf.io/v
kind: NetworkAttachmentDefinition
metadata:
annotations:
description: VLAN 2024 connection for VMs
name: vlan-
namespace: default
spec:
config: |-
{
"cniVersion":"0.3.1",
"name": "vlan-2024",
"type": "ovn-k8s-cni-overlay",
"topology": "localnet",
"netAttachDefName":"default/vlan-2024",
"vlanID": 2024,
"ipam": {}

}
---
apiVersion: k8s.cni.cncf.io/v1
kind: NetworkAttachmentDefinition
metadata:
annotations:
description: VLAN 1993 connection for VMs
name: vlan-1993
namespace: default
spec:
config: |-
{
"cniVersion":"0.3.1",
"name": "vlan-1993",
"type": "ovn-k8s-cni-overlay",
"topology": "localnet",
"netAttachDefName":"default/vlan-1993",
"vlanID": 1993,
"ipam": {}
}
Kubeletconfiguration...........................................................................................................
Thefollowingadditionalconfigurationforkubeletisappliedaftertheclusterisdeployed.

● IncreasekubeletkubeAPIBurstto 200 andkubeAPIQPSto100.Adjustingthesevalues
upfromthedefaultof 100 and50,respectively,accommodatesbulkobjectcreationon
thenodes.ThelowervaluesareusefulonclusterswithsmallernodestokeepAPIserver
resourceutilizationreasonable,howeverwithlargernodesthisisnotanissue.
● SetthemaxPodspernodeto500.Bydefault,OpenShiftsetsthemaximumPodsper
nodeto250.ThisvalueaffectsnotjustthePodsrunningcoreOpenShiftservicesand
nodefunctionsbutalsovirtualmachines.Forlargevirtualizationnodesthatcanhost
manyvirtualmachines,thisvalueislikelytoosmall.Ifyou’reusingverylargenodesand
mayhavemorethan 500 VMsandPodsonanode,thisvaluecanbeincreasedbeyond
500,howeveryouwillalsoneedtoadjustthesizeoftheclusternetwork’shostprefix
whendeployingthecluster.
● DisablenodeStatusMaxImage.Theschedulerfactorsboththecountofcontainer
imagesandwhichcontainerimagesareonahostwhendecidingwheretoplaceaPodor
virtualmachine.ForlargenodeswithmanydifferentPodsandVMs,thiscanleadto
unnecessaryandundesiredbehavior.DisablingtheimageLocalityschedulerpluginby
settingnodeStatusMaxImageto-1facilitatesbalancedschedulingacrossclusternodes,
avoidingscenarioswhereVMsarescheduledtothesamehostasaresultoftheimage
alreadybeingpresentvsfactoringinotherresourceavailability.
● Setdynamicresourceallocationforkubelet.ThedefaultCPUandmemoryreservation
forkubeletisverysmallandnotappropriatefornodeswithlargeamountsofresources,
suchashypervisorhosts.Settingdynamicresourceallocationwillsetthevaluesfor
systemreservedCPUandmemoryaccordingtothetotalamountofresourcesonthe
node.Thispreventskubeletfromstarvingforresourceswhenthenodehashighnumbers
ofPodsandvirtualmachinesrunning.
● ConfigureCPUmanagertoenablededicatedresourcesforvirtualmachinestobe
assigned.WithoutCPUmanager,virtualmachinesusingdedicatedCPUscheduling,such
asthoseconfiguredwiththecxinstancetype,cannotbescheduled.
● Configuresoftevictionthresholds.Configuringsoftevictionisvaluableforseveral
reasons,howeverthemostimportantisthatitsetstheupperboundaryformemory
utilizationonthenodesbeforethevirtualmachinesareattemptedtobemovedtoother
hostsinthecluster.Thisvalueshouldbesetforareasonablevalueaccordingtothe
approximatemaximumamountofmemoryutilizationyouwantonthenode,howeverif
allnodesareexceedingthisvaluethenworkloadwillnotberescheduled.Dependingon
theamountofmemoryinyourhosts,thiscouldbebetween90-95%.Forthesuggested
nodesizeinthisarchitecture,avalueof90%isused.
Additionalevictionthresholdsforlocalstorageutilizationcanbeconfiguredbasedon
needs,inparticularifthedisksusedbyRHCOSaresmaller(lessthan512GiB)oryou
intendtodeployephemeralvirtualmachineswithdisksstoredincontainerimages.
Belowisanexamplekubeletconfigurationtoapplytheabovesuggestedconfigurationto
computenodesinthecluster.Ifyouhavehypervisorhoststhatdonothavethe“worker”label,
anadditionalselectormaybeneeded.

apiVersion: machineconfiguration.openshift.io/v1
kind: KubeletConfig
metadata:
name: set-virt-values
spec:
machineConfigPoolSelector:
matchLabels:
pools.operator.machineconfiguration.openshift.io/worker: ""
kubeletConfig:
autoSizingReserved: true
maxPods: 500
nodeStatusMaxImages: -1
kubeAPIBurst: 200
kubeAPIQPS: 100
evictionSoft:
memory.available:"50Gi"
evictionSoftGracePeriod:
memory.available:"5m"
evictionPressureTransitionPeriod: 0s
OpenShiftDataFoundationconfiguration..................................................................
ThisreferencearchitectureutilizesODFasthestoragebackedforvirtualmachinedisks.Storage
ismodularandadditionalCSIproviderscanbeaddedtotheclustertoprovidebothODFand
third-partystoragefromothervendors.Furthermore,ODFisnotrequiredifyouchooseto
exclusivelyuseRWXstoragefromothervendors.Fornon-ODFstoragerequirements,seethe
storagesectionbelow.

● Setglobal,andvirtualizationspecific,defaultstorageclasses.Theglobaldefaultstorage
classisusedasthedefaultforPVCswhenaspecificstorageclassisnotrequested.For
thisreferencearchitecture,whenusingODFforthestorage,thesuggestedglobal
defaultisocs-storagecluster-ceph-rbd.Thevirtualizationspecificdefaultstorage
classisusedwhencreatingvirtualmachinedisks.ThedefaultwhenusingODFshouldbe
ocs-storagecluster-ceph-rbd-virtualization,unlesscreatingcustomizedstorage
classes.
● Createadditionalstorageclassesasneededtocustomizethestoragefeatures,however
ensurethattheparameters.mapOptionsvaluekrbd:rxbounceissetforanyODF
storageclassusedforVMdisks.
Multi-networkpolicyactivation...........................................................................................
TosupportnetworkpolicyforVMsconnectedtoL2networks,thefeatureneedstobeenabled
ontheOperator.Whenusingnetworkpolicytocontrolingress/egresstrafficforvirtual
machines,usetheMultiNetworkPolicyobjecttodefinerulesforaccess.

AdditionalOperatorconfiguration..............................................................................
ThebelowOperatorsaresuggestedtobedeployedandconfiguredintheclustertoprovide
additionalfunctionality.
○ NodemaintenanceOperator-Simplifiestheprocessofcordoninganddraining
nodesintheclusterformaintenancepurposes.
○ OpenShiftlogging-Collectsthelogsofclusternodes,Pods,andVMs(butnot
theguestOSlogs)intoasingleplaceforsearchingandtroubleshooting.
○ MetalLB-ProvidestheabilitytoexposePodandVM-basedapplicationsusing
anL2orL3(BGP)-basedloadbalancerhostedbyOpenShift.Thisisusefulfor
providingexternalaccesstooneormoreVMsconnectedtotheSDNthatare
hostinganapplicationusinganon-HTTP(s)(andnotusingports 80 or443)
application.
○ MigrationToolkitforVirtualization-Inadditiontoimportingvirtualmachines
fromRedHatVirtualization,RedHatOpenStack,andVMwarevSphereto
OpenShift,theMigrationToolkitforVirtualizationalsoimportsVMsfromOVA
formatandbetweenOpenShiftclusters.Additionally,virtualmachinesfromother
OpenShiftVirtualizationclusterscanbemigratedtothecurrentclusterusingthe
migrationtoolkit.
○ MigrationToolkitforContainers-UsedtomigrateVMdisks(PVCs)between
storageclasses.
○ ComplianceOperator-Fororganizationswithcompliance,e.g.HIPAAorDISA
STIG,requirements,theComplianceOperatorreports,andoptionallyapplies,
configurationtotheclustertomeetthesestandards.
○ KubeDeschedulerOperator-Thedeschedulertakesactionto(re)schedule
workloadontheclusternodeswhenconditionsaremet.Forvirtualization,thiswill
takeactionwhennodesarebelowautilizationthreshold,20%bydefault,to
rescheduleVMsfromhighlyutilizednodes,over50%bydefault,withthe
intentionofbalancingoverallnodeutilization.However,theprofileiscurrentlyin
techpreview.
○ NUMAresourcesOperator-Asthenameimplies,thisOperatorisusedwhen
NUMAawareworkloadsaredeployedtothecluster.ForVMswithlargememory
requirementsorapplicationsthatcanbenefitfromNUMA-awareschedulingand
utilizationofresources,deployingthisOperatorandassigningtheappropriate
secondaryschedulerinformsthenodeshowtomanagetheseworkloads.
○ AnsibleAutomationPlatformOperator-Forpreconfigurationofnetwork,
storage,baremetalresources,DNS,certificates,preparationofVMwareclusters
andday 2 operationstheAnsibleAutomationplatformhasvalidatedcontentthat

canhelpcustomersperformallofthesetasksandmoretodeliveranendtoend
automatedexperience.
Additionalfeatures..............................................................................................................
Compute..........................................................................................................................
OpenShiftVirtualizationintroducessupportforvirtualmachinesasanativecapabilityto
OpenShift,RedHat’senterpriseapplicationplatformofferingbasedonKubernetes.The
conceptofusingvirtualmachinesina“Kubernetesway”mayseemstrangetothosethatare
usedtotraditionalhypervisors.However,theywillfindthatmanyofthesamefeaturesand
capabilitiesthatareexpectedofanyenterpriseclassvirtualizationsolution,areavailablein
OpenShiftVirtualization.

GPU/vGPUaccess................................................................................................................
Ifyourvirtualmachinesneedtoutilize(v)GPUresources,ensurethatyoufollowthe
documentationforenablingaccesstomediateddevicesandenablingotherfeatures(IOMMU)
asneeded.

Hardware/PCIPassthrough.................................................................................................
ThePCIpassthroughfeatureallowsyoutopresentdirectaccesstohardwaredevicesofhost
machinestothevirtualgueststhemselves.Thiscanbeusefulforanumberofreasonsprimarily
relatedtoresourceisolationforthevirtualmachineandtheworkloaditishosting.Examples
includevirtualmachinesrunningAI/MLbasedworkloads,whichcanbegranteddirectaccessto
GPUdevicesonthehost,orperhapsaguestthatrequiresalargeamountofnetworkbandwidth
canbeattacheddirectlytoaphysicalnetworkadapterinsteadofsharingtheSDNwithother
virtualmachines.FormoreinformationrelatedtodevicepassthroughinOpenShift
Virtualization,seethedocumentationhere.

Network............................................................................................................................
AnOpenShiftclusterisconfiguredusinganoverlaysoftware-definednetwork(SDN)forboth
thePodandServicenetworks.Bydefault,VMsareconfiguredwithconnectivitytotheSDNand
havethesamefeatures/connectivityasPod-basedapplications.Thismeanstheyhavean
internal-onlyIPaddressforbeingaccessedacrosstheSDN,withinternalDNSresolution
providedbycreatingaServicetoabstractaccesstooneormoreVMs.OntheSDN,thevirtual

machinecanthenhaveaspecificserviceorportexposedthroughaRoutelikePods.TheMultus
ContainerNetworkInterface(CNI),whichisdeployedandusedwithallOpenShiftclusters,
enablesattachingmultiplenetworkinterfacestoPodsandVMs,includingsimultaneous
connectionstotheSDNandone-or-moreexternal(e.g.VLAN)networks.

Host-levelnetworkingconfigurationsarecreatedandappliedusingtheNMstateoperator.This
includestheabilitytoreportthecurrentconfigurationoptions,suchasbonds,bridges,and
VLANtagstohelpsegregatenetworkingresources,aswellasapplydesired-stateconfiguration
forthoseentities.

Software-DefinedNetworking(SDN)...............................................................................
AsofOpenShift4.15,theRedHat-providedSDNoptionwithOpenShiftisOVN-Kubernetes.
OVN-KubernetesisbasedonOpenVirtualNetwork(OVN)andprovidesanoverlay-based
networkingimplementation.AclusterthatusestheOVN-KubernetespluginalsorunsOpen
vSwitch(OVS)oneachnode.OVNconfiguresOVSoneachnodetoimplementthedeclared
networkconfiguration.

AdditionalSDNOptions.................................................................................................
Therearemanypartners,suchasTigera,Cisco,Citrix,andF5,whichalsohaveSDNandother
networkpluginswhichworkwithOpenShiftVirtualizationtobringadditionalfeaturesand
capabilitiestovirtualmachines.ConfirmwithyourvendorthecapabilitieswhichworkwithVMs.

NetworkingArchitecture...............................................................................................
Bydefault,OpenShiftVirtualizationwillconductmostcommunicationusingtheSDN.Thereare
twomethodsprimarilyforvirtualmachinesortheapplicationscurrentlyrunningontheclusterto
beaccessedpublicly.LikeaPod-basedapplication,aServicecanbecreated,mappedtoan
openportonthevirtualmachine,andaRouteisusedtoexposeexternalaccesstothe
application.However,thisonlyworksforHTTP(s)-basedapplicationsusingport 80 and/or443.

Forotherprotocolsandports,usingaServicewithtypeLoadBalancer,suchasMetalLBorone
frompartnerssuchasF5andCitrix,enablestheapplicationtobepubliclyaccessiblewithout
theVMbeingconnecteddirectlytoanL2(VLAN)network.Deployingandconfiguringaload
balancerproviderisapost-deploymentconfigurationoption.Therecommendedoptionforthis
referencearchitectureistouseMetalLBwithL3modeifpossible,orL2modeasafallback.

Furthermore,itisalsopossibletoconfigureadditionalnetworkresourcestoprovideaccesstoan
externalnetworksothattheVMisconfiguredwithanIPaddresswhichallowstheguest
operatingsystemtobeaccesseddirectlyusingaremoteconnectivityprotocollikeSSH.Refer
tothesectionaboveforconfiguringadditionalnetworksegmentsforhowtosetupthisscenario
withinOpenShiftVirtualization.AnsibleAutomationPlatformprovidestheabilitytoconfigure
additionalsettingsandscenarioswithintheguestoperatingsystemaswell.

FormoreinformationaboutnetworkconfigurationoptionsinOpenShift,pleaseseethe
documentationavailablehere.

Storage............................................................................................................................
OpenShiftVirtualizationusesthePersistentVolume(PV)paradigmfromKubernetestoprovide
storageforvirtualmachines.Morespecifically,eachVMdiskisstoredinadedicatedpersistent
volumeprovisionedandmanagedbyaCSI(ContainerStorageInterface)driverprovidedbyRed
Hatorasupportedstoragevendor.

CSIProvider...........................................................................................................................
Thisisdifferentfromtheparadigmusedbyothervirtualizationofferingswhereasinglestorage
device,e.g.aniSCSI/FCLUNorNFSexport,isusedtostoremanyvirtualmachinedisks.The
granularprovisioning,applicationoffeatures,control/management,andconsumptionofVM
disksenablesthestorageprovidertocustomizeandtailorthecapabilitiesoftheVMdisk
specificallytotheworkload,whichisabstractedviathestorageclasswithinOpenShift.

LivemigrationofvirtualmachinesrequiresthatallvirtualmachinedisksfortheVMbeing
migratedusePVCswiththeread-write-many(RWX)accessmode.Thisissothatthediskcan
bemountedtoboththesourceanddestinationhypervisornodeduringthemigration.While
bothblock(iSCSI,FiberChannel,etc.)andfile(NFS)protocolssupportRWXaccess,youmust
verifywithyourstoragevendoranyadditionalrequirementsorlimitationsspecifictothestorage
devicebeingusedtohostthevirtualmachine.

TheContainerStorageInterface(CSI)isastandardizedabstractionformanagingand
consumingstoragewithinKubernetes.OpenShiftVirtualizationusingthisparadigmmeansthat
storagefromanyvendorwithaCSIdriverthatmeetstheneedsandrequirementscanbeused
forvirtualmachinedisks.

Thediagrambelowprovidesahigh-leveloverviewoftheCSIcomponentsinanOpenShift
cluster.TheCSIdrivercoordinatesthecreationandmanagementofthevolumeonthestorage
device,mountingofthevolumetothenodehostingthevirtualmachine,andimplementing
featureslikesnapshotsandvolumeresizingatthestoragevolumelevel.

MultipleCSIdriverscanbedeployedtoanOpenShiftclustertoenablesupportofmany
differentstoragebackends.ThisallowsVMstousevariousdisktypes,storageprotocols,and
accessmodesfromdifferentvendorstobestsuittheworkload.

ThefollowingtabledescribestheCSIdriversthatareinstalledbydefaultinanOpenShift
cluster,dependentontheinfrastructureplatformused,alongwithwhichCSIfeaturesthey
support,suchasvolumesnapshots,cloning,andresize.

It’salsoimportanttonoteherethatnotallofthesestoragetypesareavailablewithevery
OpenShiftclusterdeployment,noraretheyallcurrentlycompatiblewithOpenShift
Virtualization.Forexample,withthe4.15release,supportforOpenShiftVirtualization
deploymentsrequirestheuseofphysicalserversdeployedon-premises,orbaremetalnodes
deployedinROSAorAWSself-manageddeployments.Othercloud-baseddeploymentsand
platformtypesareinexploratorystagesscheduledforalaterrelease.Thismeansthatasof
today,thestoragedriversfromthosecloudproviderscannotbeusedwithOpenShift
Virtualization,butonlytraditionalcontainerworkloadsinOpenShift.

InadditiontothoseCSIdriversincludedwithOpenShiftbydefaultthereareanumberof
additionalCSIdriversfrompartnerssuchasPureStorage(andPortworx),NetApp,HPE,
Dell/EMC,Hitachi,Fujitsu,andmoreavailableviaOperatorHubwithinOpenShiftorfromthe
storagevendordirectlythatsupportOpenShiftVirtualization.Checkwithyourstoragevendor
fordetailsontheirdriver’savailabilityandcapabilities.Whileeachoftheseoptionsprovidebasic
storagefunctionality,theyareoftendesignedtoleverageadvancedfeaturesofthepartner
storageplatformthatcanbeusedwithOpenShiftVirtualization.

AfewexamplesofadvancedfeaturesprovidedbyvendorCSIdriversarelistedbelow:

● StorageQOS-Resourcequotascanbesetforstorageperprojectoracrossprojectsto
ensureQoSandcontrolI/O.Usingquotasandlimitranges,clusteradministratorscanset
constraintstolimitthenumberofobjectsoramountofcomputeresourcesthatareused
inyourproject.Thishelpsclusteradministratorsbettermanageandallocateresources
acrossallprojects,andensurethatnoprojectsareusingmorethanisappropriateforthe
clustersize.FormoreinformationaboutStorageQOS,pleaseseethedocumentation
here.
● Storagemultipathing-OpenShiftVirtualizationleveragesthecapabilitiesofthe
underlyingoperatingsystem,RedHatEnterpriseLinuxCoreOS,toprovidethese
capabilities.MultipathingisnotenabledasadefaultfeatureinCoreOS,inorderto
streamlinethebasedeployedimage.Toenablethisfeature,itisrecommendedtousea
MachineConfigOperatortoenablemultipathingforoptimalstorageaccesstochoose
thebestpath.Formoreinformationaboutday 2 operations,suchasenablingmultipath
inOpenShift,pleaseseethedocumentationhere.
● Multi-hostPVCmounting-ODF,andotherCSI(seeabove)options,allowfordisk
sharinginRWX(read/writemany)accessmodeandwithCephbasedstorageclustering
ofdisks.RWXPVCsarerequiredforvirtualmachinelivemigration.
ForamatrixoffeaturesthatareprovidedbyOpenShiftVirtualization,pleaseseethefollowing
chart:

PVCsmustrequestaReadWriteManyaccessmode.
StorageprovidermustsupportbothKubernetesandCSIsnapshotAPIs
Importantconceptsandadditionalknowledge......................................................................
BusinessContinuity...........................................................................................................................
It’simportantinmanyenterprisecasestoensurethatworkloadsdeployedarehighlyavailable,
fault-tolerant,andabletoberecoveredintheeventofadisaster.HighAvailabilityinOpenShift
VirtualizationisprovidedbyOpenShiftandisbuiltintotheschedulerforpods,whichwillbe
exploredmoreindepthmomentarily.Faulttolerancecanbeaddressedinanumberofways,
fromensuringthatenoughphysicalresourcesexisttosupporttheexistingworkloadsshould
therebeaninfrastructurefailure,andthatthenodesinaclusterareorganizedinamannerto
helpinsulateitfromasinglefailurecascadingacrosstheentirecluster.Disasterrecoveryin
OpenShiftisanotherimportanttopic.Theabilitytosuccessfullybackupanapplicationand
restoreittoaremoteclusterinafunctionalmannerisrequiredformanybusinessapplications.
ThisisachievedusingRedHatnativetoolsortoolsavailablefromoneofourmanytechnological
partners.

HighAvailability..................................................................................................................................
Atahighlevel,highavailability(HA)herereferstoplatformlevelattemptstoensurean
applicationisavailableregardlessofunderlyingfailuresofthephysicalinfrastructure.An
examplescenariowhereHAcomesintoplayiswhenanodefails,forwhateverreason,and
Kubernetesthenautomaticallyreschedulesanylostpodsontothesurvivingnodesinthecluster.

VMfailure- ThisreferstoinstanceswhereindividualVMsmaycrashorotherwisefailto
performtheirtask.Thiscanoccurformanyreasons,suchasamisconfiguration;orduringa
nodedrain;ortheout-of-memoryprocessterminatestheprocessasaresultofresource
contention.Thiscanhaveanadverseeffectontheapplicationifappropriateresiliencyand
othermeasuresarenotused. Someofthestepstoensurehighavailabilityofapplicationsinthe
eventofVMfailureandterminationaredocumentedintheOpenShiftVirtualizationvirtual
machinehighavailabilityguide.

Additionally,whetheravirtualmachineisrestartedafterfailurecanbecontrolledonaper-VM
basisbysettingtheVM’srunstrategy.ThedefaultisAlways,whichwillensurethattheVMis
alwaysrunning,unlessit’sshutdownviaOpenShift.ThismeansthatiftheVMisshutdownfrom

withintheguestoperatingsystem,itwouldresultintheVMbeingpoweredbackon.Tohavethe
VMstaypoweredoffafterbeingshutdownfromwithintheguestoperatingsystem,usethe
RerunOnFailurepolicy.NotethateitherofthesewillresultintheVMbeingrestartedaftera
nodefailureeventifitwaspreviouslyrunning.

Foradditionalinformation,pleaseseethisdocumentation.

Nodefailure- Referstoanytimeanodeunexpectedlybecomesunavailabletothecluster.This
couldbebecauseofaphysicalhardwarefailure,oranenvironmentalissuesuchasapower
failure.Ineithercase,thenodebecomesinaccessibleonthenetworkandworkloadsmustbe
rescheduledforothernodesthatareavailable.Inthiscase,somepotentialwaystomitigatethe
failureare:

● Havehealthchecksinplaceforthenodes.Thesewilldetectwhenanodehasfailedin
ordertotriggerthenextphaseofrecovery:fencing.
● Understandthat,bydefault,ittakesfiveminutesforKubernetestoidentifyand
rescheduleworkloadsfromanunreachablenode.Forworkernodesthathavebeen
provisionedusinganIPI-basedinstall,asisdonewiththisarchitecture,youmayreduce
thistimebyhavingnodehealthchecksinplace,whichwillresultinthenodebeing
fenced.IfnoIPMI/BMCconnectivityisavailable,usetheSelfNodeRemediation
Operatortodetectnodefailurefaster.
● Fencingreferstotheprocessofensuringthatthelostnodeisnolongerrunningany
workload.Withinthisreferencearchitecture,theFencingAgentsRemediationmethodis
usedtopowercyclethephysicalserverusingIPMI/BMCintegration.
Clusterfailure- Clusterhighavailabilitydependsonthecontrolplanestayinghealthyinthe
eventoffailure.Ifthecontrolplanefails,orotherwisebecomesinaccessible,newworkloadwill
beunabletoscheduleandthecomputenodesmaybegintoshutdownVMsafteratimeout.The
schedulingofPodsisoneofthemostcriticalfunctionsprovidedbythecontrolplane.With
OpenShift4,allclusters(withtheexceptionofsinglenodeOpenShift)havethreecontrolplane
nodes,eachonealsohostinganetcdmember.Withthreenodes,thecontrolplanewillcontinue
tofunctionevenwithonefailednode.Iftwonodesfail,thenthecontrolplanecannotprocess
clusterchanges,andgoesintoaread-onlymode.IfallthreeOpenShiftcontrolplanenodes
happentofailatthesametimethentheclusterisofflineandunreachableuntiltheclustercan
berestoredtoworkingorder.

DisasterRecovery...............................................................................................................................
Ingeneral,ifseparateavailabilityzonescannotbeguaranteedfortheclustercontrolplaneand
workernodestoassistwithmitigationofasitefailure,thenthebestoptionavailableistodeploy
additionalOpenShiftclustersacrossmultiplesitesandestablishadisasterrecoverypolicy.This
policyshoulddefinewhichactionsneedtobetakentorecoverapplicationsshouldtherebea
clusterfailingdisasteratonesite.Insomecasesthiscanbeachievedbyplacingaglobalload
balancerinfrontofanumberofclustersatremotesites,ensuringthatoneclustercanstill
serviceapplicationsshoulditspartnerfail.However,thisdoesrequiresomelevelofapplication
awarenessandmonitoringtoensurethatthecorrectinstanceisactiveandanypersistentdata
usedbytheapplicationisreplicatedappropriately.

Toachievethis,RedHathasmadeavailabletheOpenShiftAPIsforDataProtection(OADP)as
anoperatorinOpenShifttoenableuserstocreatebackupsandrestoreworkloadinOpenShift.
FormoredetailsonOADP,pleaseseethedocumentationavailablehere.

ThespecificstorageproductusedforOpenShiftVirtualizationmayoffertheabilitytoreplicate
orotherwiseprotectthevirtualmachinedisksandmetadata.Thedisasterrecoveryguideoffers
severaloptionsforhowtoprotectandrecovervirtualmachineswithvaryingrecoverytime
object(RTO)andrecoverypointobject(RPO)goals.

Additionaloptionsforbackup,restore,anddisasterrecoveryexistbymakinguseofproducts
fromanumberofRedHatPartnerswhichprovidetheabilitytobackupandrestorestateful
applicationsfromoneOpenShiftclustertoanother.Severalofthesepartnersfocusonbackup
andrecoveryforapplicationsinOpenShiftandalsocurrentlysupportOpenShiftVirtualization
workloads.Inadditiontothesepartnersthatfocusexclusivelyonbackupandrecovery
operations,therearestoragevendorswithnativebackupfunctionalitythatalsosupport
OpenShiftVirtualization.

Hereisalistingofsomepartnersthatprovidethisfunctionalityofthese:
● KastenK10byVEEAM
● TrilioforKubernetes
● StorwareBackupandRecovery
● PortworxPX-BackupandMetro-DR
Scalability............................................................................................................................................
TheupwardboundsofanOpenShiftVirtualizationdeploymentisbasedonthetestedand
supportedmaximumsofRedHatOpenShiftitself.Thetablebelowprovidesthebasic
informationforscalingofresourcesavailabletoasinglecluster.MoreinformationonOpenShift
testedmaximumscanbefoundinthedocumentationhere,virtualizationspecificmaximumscan
befoundhere.

Component TestedMaximum
Nodesinacluster 500 [1][2]
VirtualMachinesinacluster 10,000
CPUsperVirtualMachine 216 [3]
RAMperVirtualMachine 16 TB[4]
SingleDiskSizeperVirtualMachine 2 TB[5]
[1]ThisnumberislowerthantheoveralllimitationofOpenShift
[2]While 500 nodeclustersaretested,itissuggestedtorunclustersizesoflessthan 100 nodes
[3]Theoreticallimitis 710 onx86_64infrastructure,duetounderlyingconstraintsofKVMandRHCOS
[4]LimitedbasedonunderlyingconstraintsofKVMandRHCOS
[5]Thisisthetestedsize,thesupportedmaximumisthesameasRHEL.

SubscriptionInformation...................................................................................................................
OpenShiftVirtualizationisincludedinallOpenShiftsubscriptions.Subscriptionmodelsforbare
metalarepurchasedbasedonthesocketcountinaphysicalnode,andasinglesubscriptionis
allocatedto1-2sockets,withupto 64 corestotal.AbaremetalsystemrunningOpenShift
VirtualizationthenhastheabilitytorunanunlimitednumberofvirtualizedRedHatEnterprise
Linux(RHEL)guestoperatingsysteminstancesincludedasapartofthatsubscription.While
thissubscriptioncoversRHELguests,itdoesnotcovervirtualizedOpenShiftclustershostedby
thephysicalOpenShiftVirtualizationcluster.OpenShiftclustershostedeitherinVMs,or
deployedusingthehostedcontrolplanefeature,areentitledbyusingcore-basedsubscriptions,
whichisthesameasOpenShiftdeployedonanyhypervisorplatform.

SocketSubscriptionRequired CoreSubscriptionRequired
OpenShiftbaremetal Upto 64 physicalcores, 1 or 2
sockets.*
N/A
VirtualOpenShifton
OpenShiftbaremetal
N/A VirtualOpenShiftentitledby
computenodevCPU.**
Mixedvirtualmachineson
OpenShiftbaremetal
Upto 64 physicalcores, 1 or 2
sockets.*
VirtualOpenShiftentitledby
computenodevCPU.**
*Hardwarewithgreaterthan 64 physicalcoreswillrequireadditionalsubscriptionspernode.
**MaximumcoresubscriptionforvirtualOpenShiftcappedasphysicalcorelimitofhostcluster.

SolutionSummary.....................................................................................................................
OpenShiftVirtualizationisafullydevelopedvirtualizationsolutionutilizingthetype-1KVM
hypervisorfromRedHatEnterpriseLinuxwiththepowerfulfeaturesandcapabilitiesof
OpenShiftformanaginghypervisornodes,virtualmachines,andtheirconsumers.Byprovidinga
singlecontrolplaneformanagingbothcontainersandVMs,OpenShiftcanstreamlinemany
administrativeactionsandassistwithexpeditingthedeploymentoffullapplicationstacksthat
involvebothoftheseresourcetypes.Customersshouldalsobeassuredofthevalueprovidedby
OpenShiftVirtualizationincomparisontootherenterprisevirtualizationsolutions,asitalso
maintainsmanyofthesamehypervisorfeaturesexpectedofsuchasolution,likelivemigration,
virtualguestcloning,andintegrationwiththird-partytoolsforvirtualmachinemanagement.

AdditionalLinksandReferences.............................................................................................
Documentation:..................................................................................................................................
AboutOpenShiftVirtualization:
https://docs.openshift.com/container-platform/4.15/virt/about_virt/about-virt.html

Videos:.................................................................................................................................................
IntheCloudsEpisode33:https://www.youtube.com/watch?v=qzrTfJfbb8o

AskanOpenShiftAdminEpisode89:https://www.youtube.com/watch?v=oUm7yftZI20

AppendixA.Detailedresourcecalculations...........................................................................
CPUcapacitycalculation..................................................................................................................
(((physical_cpu_cores - odf_requirements - control_plane_requirements) *
node_count * overcommitment_ratio) * (1 -ha_reserve_percent)) * (1 -
spare_capacity_percent)

● physical_cpu_cores=thenumberofphysicalcoresavailableonthenode.
● odf_requirements=theamountofresourcesreservedforODF.Avalueof 32 coreswas
usedfortheexamplearchitectures.
● control_plane_requirements=theamountofCPUreservedforthecontrolplane
workload.Avalueof 4 coreswasusedfortheexamplearchitectures.
● node_count=thenumberofnodeswiththisgeometry.Forsmall,allnodeswereequal.
Formedium,thenodesaremixed-purpose,sothepreviousstepswouldneedtobe
repeatedforeachnodetype,takingintoaccounttheappropriatenodetype.
● overcommitment_ratio=theamountofCPUovercommitment.Aratioof4:1wasused
forthisdocument.
● spare_capacity=theamountofcapacityreservedforspare/burst.Avalueof10%was
usedforthisdocument.
● ha_reserve_percent=theamountofcapacityreservedforrecoveringworkloadlostin
theeventofnodefailure.Forthesmallexample,avalueof25%wasused,allowingfor
onenodetofail.Forthemediumexample,avalueof20%wasused,allowingfortwo
nodestofail.
Memorycapacitycalculation............................................................................................................
((total_node_memory - odf_requirements - control_plane_requirements) *
soft_eviction_threshold_percent * node_count) * (1 - ha_reserve_percent)

● total_node_memory=thetotalphysicalmemoryinstalledonthenode.
● odf_requirements=theamountofmemoryassignedtoODF.Avalueof72GiBwasused
fortheexamplearchitecturesinthisdocument.
● control_plane_requirements=theamountofmemoryreservedforthecontrolplane
functions.Avalueof24GiBwasusedfortheexamplearchitectures.
● soft_eviction_threshold_percent=thevalueatwhichsoftevictionistriggeredto
rebalanceresourceutilizationonthenode.Unlessallnodesintheclusterexceedthis
value,it’sexpectedthatthenodewillbebelowthisutilization.Avalueof90%wasused
forthisdocument.
● node_count=thenumberofnodeswiththisgeometry.Forsmall,allnodeswereequal.
Formedium,thenodesaremixed-purpose,sothepreviousstepswouldneedtobe
repeatedforeachnodetype,takingintoaccounttheappropriatenodetype.
● ha_reserve_percent=theamountofcapacityreservedforrecoveringworkloadlostin
theeventofnodefailure.Forthesmallexample,avalueof25%wasused,allowingfor
onenodetofail.Forthemediumexample,avalueof20%wasused,allowingfortwo
nodestofail.
ODFstoragecapacitycalculation...................................................................................................
(((disk_size * disk_count) * node_count) / replica_count) * (1 -
utilization_percent)

● disk_size=thesizeofthedisk(s)used.4TBand8TBdiskswereusedintheexample
architectures.
● disk_count=thenumberofdisksofdisk_sizeinthenode.
● node_count=thenumberofnodeswiththisgeometry.Forsmall,allnodeswereequal.
Formedium,thenodesaremixed-purpose,sothepreviousstepswouldneedtobe
repeatedforeachnodetypetakingintoaccounttheappropriatenodetype.
● replica_count=thenumberofcopiesODFstoresofthedataforprotection/resiliency.A
valueof 3 wasusedforthisdocument.
● utilization_percent=thedesiredthresholdofcapacityusedintheODFinstance.Avalue
of65%wasusedforthisdocument.
AppendixB.Changelog...........................................................................................................
May 2024 - version1.0.0
● Initialrelease.
