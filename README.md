# qBOLD_modeling

This script provides functions that calculate and generate 1D or 3D maps of important parameters in qBOLD signaling. The parameters of interest include M, N, alpha, CMRO2, CBF, BOLD etc. 

### List of functions

#### Read Input

to1D(listOf3DArray, listOfName, listOfCond)

AverageFileReadandFilter(fileName, cond, IDs = None, thres = 0, rename = True)

#### Output 3D map

to3D(df, listOfColumnName, dimension)

#### Specify the range of calculation

getRange(listOf3DCoord, indexMap)

getRegion(df, region, regionType)

#### Precalculation

preparation(cond, TE)

#### Calculation

applyFuncToDf(func, df, applyRange = [], TE = None, a = None, b = None, m = None, relCMRO2 = False, relCBF = False)

<br/>


### Details

#### to1D(listOf3DArray, listOfName, listOfCond)

This function takes 3D maps of different parameters and convert the data into a pandas dataframe for further calculation

##### Input: 

listOf3DArray: A list of 3D maps of relevant data stored in numpy 3D arrays

listOfName: A list of the name of the parameters of the 3D maps. Accepted values are: "CBF","OEF","R2p","BOLD","CMRO2","lobe", "nw", "CBV". if other names are added, a warning but no error will be given. The script cannot use columns with other names, but it will keep them in the dataframe for your own interest.

listOfCond: Condition of the 3D maps. For comparative maps such as BOLD, please use the concatenated string of treatment and control conditions. e.g.: if you are comparing "Calc" condition with "Rest" condition, please put "CalcRest" the BOLD map. The script is case-sensitive. Capitalisation of the first letter is recommended but not required.

All three list should have the same length, and the arrays in listOf3DArray should have the same dimention

##### Output:

a dataframe, a tuple, and a 3D array

Dataframe: A pandas dataframe with each voxel as a row and each parameter in the listOf3DArray as a column. This df will be used for downstream calculation.

Tuple: Dimension of the 3D arrays. This allows you to double check the input. The output can also be used as the dimension input in the to3D() function.

3D array: An index map mapping 3D index to row index in the dataframe. This can be used as the indexMap input in the getRange() function.

<br/>

#### AverageFileReadandFilter(fileName, cond, IDs = None, thres = 0, rename = True)

This function takes the output of the qBOLD script, filter the data according to the participant ID, conditions and vox counts, and rearrange the data into a pandas dataframe that is suitable for downstream analysis.

##### Input:

fileName: Directory of the qBOLD output csv file

cond: A list of two conditions of interest. e.g. ["calc", "control"]. Please make sure that the conditions match the cond entries of the csv file

IDs: A list of integers representing the ID of participants of interest. Default is None. If default is used, all participants will be included.

thres: The minimum number of voxel count for a parcel to be included in the analysis. Default is 0.

rename: Rename the columns for downstream analysis. This will only work for the output of Samira's qBOLD script. Default is true. Renaming the columns is necessary for downstream analysis. If you set rename = False, please rename the column names yourself to variable name ("CBF, "CBV", etc) + condition with no space or underscore in between.

##### Output:

A pandas dataframe with each subject's parcel as  row, and each distinct entity in the par column of the original csv as a column. The rows with incomplete data after filtering will be removed.

<br/>

#### to3D(df, listOfColumnName, dimension)

This function generates 3D maps for calculated parameters.

##### Input:

df: The pandas dataframe storing the calculated values

listOfColumnName: A list of column names for the parameters that 3D maps will be generated.

dimension: A tuple of length 3 specifying the dimension of the 3D map. The second output of to1D() function can provide this tuple.

##### Output:

A dictionary with keys being the column names and values being the corresponding 3D maps.

<br/>

#### getRange(listOf3DCoord, indexMap)

This function takes a list of 3D indices and converts them to 1D row indices for the dataframe. This is used to perform a calculation on a specific set of voxels.

##### Input:

listOf3DCood: A list of tuples representing the 3D indices of the voxels of interest

indexMap: A pre-generated 3D array map. The third output of to1D can provide this index map

##### Output:

A list of row numbers in the dataframe corresponding to the voxels of interest. This can be used as the applyRange input for applyFuncToDf().

<br/>

#### getRegion(df, region, regionType)

This function gives the row indices corresponding to a specific brain region.

##### Input:

df: The pandas dataframe storing the data

region: A string specifying the region of interest, e.g. "Occ". The region must be an entity in either the nw or lobe column.

regionType: Either "nw" or "lobe"

##### Output:

A list of row numbers in the dataframe corresponding to the region of interest. This can be used as the applyRange input for applyFuncToDf().

</br>
