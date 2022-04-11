Conques Internes SWAT+ model

________________________________________________________________________________________


26/03/22, v05 (ci_05)

	QSWAT+ v2.0.6
	SWAT+ Editor v2.0.4
	SWAT+ rev60.5.4

Projecció de les capes: EPSG:25831 - ETRS89 / UTM zone 31N - Proyectado

-------------------------

Step 1: Delineate Watershed

Upload DEM

Use existing watershed: subbasins.shp, wshed.shp, channel.shp, reservoirs.shp (inlets/outlets shp in "...\2- Predefined watershed")

Recalculate and overwrite existing -> Run -> OK

Copy the raster floodplain100tgeo.tif from 2- Predefined watershed -> current version to the project folder (Watershed -> Rasters -> Landscape -> Flood


-------------------------

Step 2: Create HRUs

     Landuse and Soil:

Landuse map: Corine
Soil map: European database

Slope bands [0, 2, 8, 9999]
Select floodplain map -> floodplain100tgeo2.tif
Change SRC of slope raster to EPSG:25831
Elevation bands NO

Read choice: Read from maps
Short channel merged 0 ha

Select Generate FullHRUs shapefile -> Read

	Subbasins count: 34
	Channels count: 1185
	Full HRUs count: 35236

Exempt landuses: ALFA, BARL, CORN, CRIR, ORCD & RICE

Single/Multiple HRUs: 
select: Filter by landuse/soil/slope
	Landuse: 6%
	Soil: 10%
	Slope: 12%

Create HRUs -> 16581 (16575 normal HRUs and 6 reservoirs)
-------------------------

Step 3: Edit Inputs and Run SWAT

Import project from QSWAT
 
   EDIT SWAT+ INPUTS

	CLIMATE-Weather Generator

Impor weather generator from 'swatplus_wgn_aemet_spain02.sqlite'.
Select Table name in database: wgn_aemet_spain02
Mark box 'using observed weather data'

	CLIMATE-Weather Stations

Import data from stations, use SWAT2012 format.
Weather Stations -> Import Data -> browse folder "...\5-Weather" and Start import

	DECISION TABLES-Land Use Management

Import lum.dtl from folder "...\6- To import\Decision tables" -> 25 rows

	DECISION TABLES-Reservoir Release

Import res_rel.dtl from folder "...\6- To import\Decision tables" -> 166 rows

	INITIALIZATION DATA-Organic Mineral 

Create the following records and only modify the Volume of water:

init1: 1.48008
init2: 1.869
init3: 2.2868
init4: 0.60197
init5: 2.53805
init6: 0.78105

	CONNECTIONS-Reservoirs-Initial

Create the following records and add to Organic Mineral Properties:

resinit1: init1
resinit2: init2
resinit3: init3
resinit4: init4
resinit5: init5
resinit6: init6

	CONNECTIONS-Reservoirs

Change the Initial Properties and the Release Decision Table for each reservoir:

res1: resinit1, darnius_boadella
res2: resinit2, foix
res3: resinit3, baells
res4: resinit4, llosa
res5: resinit5, sant_pons
res6: resinit6, sau_susqueda

	CONNECTIONS-Reservoirs-Reservoir Hydrology

Modify the following:

Name     area_ps      vol_ps     area_es     vol_es
res1   167.80319     2095.56   236.33742       6110
res2      51.338         300    58.12404        374
res3   182.90462      3953.4   326.37832      10943
res4   178.16175      4862.4   263.12157       8000
res5    91.42635       754.2   117.02445       2438
res6    696.2718  23736.8175        1045      39870

	LAND USE MANAGEMENT-Management Schedules

Add the following records/operations to existing records:

alfa_rot: add automatic schedules hay_cutting, irr_str8 and fert_rest
barl_rot: add automatic schedules irr_str8 and fert_rest
corn_rot: add automatic schedules irr_str8 and fert_rest
crdy_rot: add automatic schedules pl_hv_crdy and fert_rest
crgr_rot: add automatic schedule fert_rest
crir_rot: add automatic schedules pl_hv_crir, irr_str8 and fert_rest
grap_rot: add automatic schedule fert_grap and operation harvest only, month September day 1. Plant name grap and harvest operation name orchard
oliv_rot: add automatic schedule fert_oliv and operation harvest only, month November day 1. Plant name oliv and harvest operation name orchard
orcd_rot: add automatic shedules irr_str8 and fert_orcd, and operation harvest only, month September day 1. Plant name orcd and harvest operation name orchard
rice_rot: add automatic schedules irr_str8 and fert_rest

	LAND USE MANAGEMENT-Land Use Management

Add alfa_rot, crdy_rot, crir_rot, grap_rot, oliv_rot and orcd_rot to management sch. column in alfa_lum, crdy_lum, crir_lum, grap_lum, oliv_lum and orcd_lum, respectively.

	LAND USE MANAGEMENT-Operations Databases-Irrigation

Edit sprinkler_high to sprinkler_hig


	INITIALIZATION DATA-Plant communities

alfa_comm -> Edit -> alfa -> Edit: set Land cover status to yes, Initial leaf area index to 2, Initial biomass to 2000 and Fraction of years to maturity to 1 -> Save changes
bsvg_comm -> Edit -> bsvg -> Edit: set Land cover status to yes, Initial leaf area index to 1.5, Initial biomass to 500 and Fraction of years to maturity to 1 -> Save changes
gras_comm -> Edit -> gras -> Edit: set Land cover status to yes, Initial leaf area index to 2, Initial biomass to 10000 and Fraction of years to maturity to 1 -> Save changes
past_comm -> Edit -> past -> Edit: set Initial leaf area index to 2 -> Save changes
shrb_comm -> Edit -> shrb -> Edit: set Land cover status to yes, Initial leaf area index to 2, Initial biomass to 10000 and Fraction of years to maturity to 1 -> Save changes
wehb_comm -> Edit -> wehb -> Edit: set Land cover status to yes, Initial leaf area index to 2, Initial biomass to 20000 and Fraction of years to maturity to 1 -> Save changes
wetw_comm -> Edit -> wetw -> Edit: set Land cover status to no, Initial leaf area index to 0, Initial biomass to 0 and Fraction of years to maturity to 0 -> Save changes

	DATABASES-Plants

Search wetw -> Edit: change to 0 from Biomass-energy ratio to Light extinction coefficient except Maximum (potential) leaf area index, set to 0.5 (same parameters as those for WATR in previous versions of SWAT+, since the current one changes the WATR tiles not used for a reservoir to WETW (wetland_wet), but we want to keep it as "water". 
   
	DATABASES-Fertilizer

Create the following record:

   Name: cic_fr
   Fraction of fertilizer that is mineral N (NO3+NH3): 0.269329318850542
   Fraction of fertilizer that is mineral P: 0.0807987956551625
   Fraction of fertilizer that is org N: 0.499901450380227
   Fraction of fertilizer that is org P: 0.149970435114068
   Fraction of mineral N content of fertilizer that is NH3: 0.99


-------------------------

Before writing the TxtInOut, open the ci_05.sqlite database (e.g. with DB Browser for SQLite)
	
	1. Import the table "recall_con_out.csv" from the folder "...\6- To import\Database tables" and add records to current table.
	2. Import point sources data for urban supply "recall_dat.csv" from from the folder "...\6- To import\Database tables". Open Advanced and in Conflict strategy select Replace existing row. 

Write changes, close the database, and go back to the SWAT+ Editor.

	3. Import point sources data for wwtp and industrial discharges (aplicació Adrià)

-------------------------

   RUN SWAT+ 

	Set your simulation period (based on observed files, default)

Starting date of simulation: 1 de enero de 2000
Ending date of simulation: 14 de marzo de 2021
Time steps in a day for rainfall, runoff and routing: Daily

	Choose output to print (as needed)

Warm-up period: 1

	SIMULATION-Time
Number of years to not print output: 1
Beginning Julian Day of simulation to start printing output files for daily printing only: 0
Beginning year of simulation to start printing output files: 0
Ending Julian Day of simulation to start printing output files for daily printing only: 0
Ending year of simulation to start printing output files: 0
Daily print within the period (e.g., interval=2 will print every other day): 1

-------------------------

TxtInOut:

Copy the following files from the folder "...\7- Irrigation\Files to add-modify in the TxtInOut" to the TxtInOut folder.
	
	file.cio: modified to accept water_allocation.wro and calibration.cal
	water_allocation.wro: irrigation districts
	Rev_60_5_4_mod_rel.exe: new executable to run the reservoir operations and the irrigation

Copy the file "calibration.cal" from "...\9- Calibration results" to the TxtInOut folder.

