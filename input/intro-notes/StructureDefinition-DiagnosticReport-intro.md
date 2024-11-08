## Mappings and Model

Based on [Royal College of Radiologists - Understanding the Technical Options](https://www.rcr.ac.uk/media/wwtp2mif/rcr-publications_radiology-reporting-networks-understanding-the-technical-options_march-2022.pdf) mapped to FHIR following [HL7 Version 2 to FHIR - ORU_R01 Unsolicited Report Alarm](https://build.fhir.org/ig/HL7/v2-to-fhir/ConceptMap-message-oru-r01-to-bundle.html).

Note: performer and resultsIntepreter links to HL7 FHIR Practitioner has removed several HL7 v2 fields, and just retained identifier and name. It is expected these values would be available via the NHS England Healthcare Worker API which may conform to [IHE Mobile Care Services Discovery (mCSD)](https://profiles.ihe.net/ITI/mCSD/index.html) interface contracts.

| Name                                                                                         | HL7 FHIR DiagnosticReport                                                                                | HL7 v2 Segment and Name                                     | DICOM                                                                        | Note                                                                                                                                                                                                                                                                                                                                                                                                                 | NHS Data Dictionary                                                                                                                                                            |
|----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Report Id                                                                                    | identifier.value                                                                                         | OBR-3 Filler Order Number                                   |                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
| Patient                                                                                      | subject.identifier.value                                                                                 | PID-3-1   Patient Identifiers                               |                                                                              | Where PID-3-4 (Assigning Authority) = NHS                                                                                                                                                                                                                                                                                                                                                                            | [NHS NUMBER](StructureDefinition-nhs-number.html)                                                                                                  |
|                                                                                              | subject.identifier.system                                                                                | PID-4-4 Assigning Authority                                 |                                                                              | Fixed value `https://fhir.nhs.uk/Id/nhs-number`                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |        
|                                                                                              | subject.display                                                                                          | PID-5      Patient Name                                     |                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
| Accession Number                                                                             | basedOn.identifier.value                                                                                 | ORC-3      Filler Order Number                              | 0008,0050 AccessionNumber                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                      | [RADIOLOGICAL ACCESSION NUMBER](StructureDefinition-accession-number.html)                                                                                                                                                                                |
|                                                                                              | basedOn.identifier.system                                                                                | ORC-3-4 Assigning Authority                                 | 0008,0051 Assigning Authority                                                | Convert to a FHIR System Uri                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                |
|                                                                                              | basedOn.type                                                                                             |                                                             |                                                                              | Fixed value `ServiceRequest`                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                |
| Visit Number                                                                                 | encounter.identifier.value                                                                               | PV1-19     Visit Number                                     |                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
| NICIP                                                                                        | code                                                                                                     | OBR-4 	Universal Service Identifier                         | 0008,1032                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                      | [IMAGING CODE (NICIP)](ValueSet-imaging-code-nicip.html)                                                                                                                       |                                                                                                                                                                                 
| Modality                                                                                     | category                                                                                                 | OBR-24 	Diagnostic Serv Sect ID                             | 0008,0024 [Modality](ValueSet-modality.html)                                 |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
| Exam Completion Date                                                                         | effectiveDateTime                                                                                        | OBR-7	Observation Date/Time (if OBR-8 not present)          |                                                                              | Exam completion date+time by radiographer                                                                                                                                                                                                                                                                                                                                                                            |                                                                                                                                                                                |
|                                                                                              | effectivePeriod                                                                                          | OBR-7 Observation Date/Time OBR-8 Observation End Date/Time |                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
| Finalised Report Issued                                                                      | issued                                                                                                   | OBR-22 Results Rpt/Status Chng – Date/Time                  | DICOM SR Structured Reporting Object                                         |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
|                                                                                              | performer(organisation).identifier.value                                                                 | PV1-3-4	Assigned Patient Location - Facility                | 0008,0082                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                      | [SITE CODE (OF IMAGING)](https://www.datadictionary.nhs.uk/data_elements/site_code__of_imaging_.html?hl=site%2Ccode) This is referring to Location codes, ODS codes preferred? | 
|                                                                                              | performer(organisation).identifier.system                                                                |                                                             |                                                                              | Fixed value `https://fhir.nhs.uk/Id/ods-organisation-code`                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                |
|                                                                                              | performer(organisation).type                                                                             |                                                             |                                                                              | Fixed value `Organization`                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                |
| Operator                                                                                     | performer(practitioner).identifier.value                                                                 | OBR-34-1	Technician                                         | 0008,1049 and/or 0008,1070                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                      | See [CONSULTANT CODE](https://www.datadictionary.nhs.uk/data_elements/consultant_code.html) for formats of GMC, HCPC and NMC codes                                             | 
|                                                                                              | performer(practitioner).identifier.system                                                                |                                                             |                                                                              | See NHS England [FHIR Practitioner](https://simplifier.net/guide/NHSDigital/Home/FHIRAssets/AllAssets/Profiles/NHSDigital-Practitioner.guide.md?version=current) identifier guidance. This link is deprecated, find replacement                                                                                                                                                                                      |                                                                                                                                                                                |
|                                                                                              | performer(practitioner).display                                                                          | OBR-34-2 Surname                                            |                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
|                                                                                              | performer(practitioner).type                                                                             |                                                             |                                                                              | Fixed value `Practitioner`                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                |
|                                                                                              | performer(practitioner).<br/>extension(performerFunction)<br/>.valueCodeableConcept.coding.code          |                                                             |                                                                              | Fixed value `SPRF`                                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |
| Primary Reporter                                                                             | resultsInterpreter(practitioner).identifier.value                                                        | OBR-32-1 Principal Result Interpreter                       | 0008,1062                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                      | See [CONSULTANT CODE](https://www.datadictionary.nhs.uk/data_elements/consultant_code.html) for formats of GMC, HCPC and NMC codes                                             | 
|                                                                                              | resultsInterpreter(practitioner).identifier.system                                                       |                                                             |                                                                              | See NHS England [FHIR Practitioner](https://simplifier.net/guide/NHSDigital/Home/FHIRAssets/AllAssets/Profiles/NHSDigital-Practitioner.guide.md?version=current) identifier guidance. This link is deprecated, find replacement                                                                                                                                                                                      |                                                                                                                                                                                |
|                                                                                              | resultsInterpreter(practitioner).display                                                                 | OBR-32-2 Surname                                            |                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
|                                                                                              | resultsInterpreter(practitioner).type                                                                    |                                                             |                                                                              | Fixed value `Practitioner`                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                |
|                                                                                              | resultsInterpreter(practitioner).<br/>extension(performerFunction)<br/>.valueCodeableConcept.coding.code |                                                             |                                                                              | Fixed value `PPRF`                                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |
| Secondary Reporter                                                                           | resultsInterpreter(practitioner).identifier.value                                                        | OBR-33-1 Assistant Result Interpreter                       | 0008,1062                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                      | See [CONSULTANT CODE](https://www.datadictionary.nhs.uk/data_elements/consultant_code.html) for formats of GMC, HCPC and NMC codes                                             | 
|                                                                                              | resultsInterpreter(practitioner).identifier.system                                                       |                                                             |                                                                              | See NHS England [FHIR Practitioner](https://simplifier.net/guide/NHSDigital/Home/FHIRAssets/AllAssets/Profiles/NHSDigital-Practitioner.guide.md?version=current) identifier guidance. This link is deprecated, find replacement                                                                                                                                                                                      |                                                                                                                                                                                |
|                                                                                              | resultsInterpreter(practitioner).display                                                                 | OBR-33-2 Surname                                            |                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
|                                                                                              | resultsInterpreter(practitioner).type                                                                    |                                                             |                                                                              | Fixed value `Practitioner`                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                |
|                                                                                              | resultsInterpreter(practitioner).<br/>extension(performerFunction)<br/>.valueCodeableConcept.coding.code |                                                             |                                                                              | Fixed value `SPRF`                                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |
| Radiographer comments                                                                        |                                                                                                          | NTE-3                                                       |                                                                              | See note below about FHIR Document                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |
|                                                                                              | conclusion                                                                                               |                                                             | Is this present?                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |  
| Radiology Report Narrrative comment<br/>Radiographer comments<br/>Narrative clinical history | presentForm (Entire Report)                                                                              | OBX-5                                                       | [DICOM SR](https://www.dicomstandard.org/News-dir/ftsup/docs/sups/sup23.pdf) | If text based, the multiple types here may be better suited to a [FHIR Document](https://hl7.org/fhir/R4/documents.html), see [HL7 Europe Laboratory Report](https://build.fhir.org/ig/hl7-eu/laboratory/). Note DICOM SR and FHIR Documents are similar concepts. <br/><br/> See also [IHE Interactive Multimedia Report (IMR)](https://profiles.ihe.net/RAD/IMR/volume-1.html#1522-imr-actor-options) for options. | 

