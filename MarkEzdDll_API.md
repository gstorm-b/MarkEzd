# MarkEzdDll API Summary

This document summarizes the constants, structs, and function-pointer APIs declared in `MarkEzdDll.h`.

## Overview

- The header mainly defines `typedef` function pointers for the EZCad / LMC1 SDK.
- Most APIs return an `int` status code and use the `LMC1_ERR_*` constants below.
- The header also defines hatch, barcode, DataMatrix, and font-related constants.

## Status Codes

| Code | Name | Meaning |
| --- | --- | --- |
| 0 | `LMC1_ERR_SUCCESS` | Success |
| 1 | `LMC1_ERR_EZCADRUN` | EZCad is already running |
| 2 | `LMC1_ERR_NOFINDCFGFILE` | `EZCAD.CFG` not found |
| 3 | `LMC1_ERR_FAILEDOPEN` | Failed to open LMC1 |
| 4 | `LMC1_ERR_NODEVICE` | No valid LMC1 device found |
| 5 | `LMC1_ERR_HARDVER` | Hardware version mismatch |
| 6 | `LMC1_ERR_DEVCFG` | Device configuration file not found |
| 7 | `LMC1_ERR_STOPSIGNAL` | Alarm / stop signal |
| 8 | `LMC1_ERR_USERSTOP` | Stopped by user |
| 9 | `LMC1_ERR_UNKNOW` | Unknown error |
| 10 | `LMC1_ERR_OUTTIME` | Timeout |
| 11 | `LMC1_ERR_NOINITIAL` | SDK not initialized |
| 12 | `LMC1_ERR_READFILE` | File read error |
| 13 | `LMC1_ERR_OWENWNDNULL` | Owner window is null |
| 14 | `LMC1_ERR_NOFINDFONT` | Font not found |
| 15 | `LMC1_ERR_PENNO` | Invalid pen number |
| 16 | `LMC1_ERR_NOTTEXT` | Entity is not a text object |
| 17 | `LMC1_ERR_SAVEFILE` | Failed to save file |
| 18 | `LMC1_ERR_NOFINDENT` | Entity not found |
| 19 | `LMC1_ERR_STATUE` | Operation not allowed in current state |

## Related Constants

### Hatch Attributes

| Name | Value | Meaning |
| --- | --- | --- |
| `HATCHATTRIB_ALLCALC` | `0x01` | Treat all entities as one combined shape for hatch calculation |
| `HATCHATTRIB_EDGE` | `0x02` | Trace the contour once |
| `HATCHATTRIB_BIDIR` | `0x08` | Bidirectional hatch |
| `HATCHATTRIB_LOOP` | `0x10` | Loop / ring hatch |

### Barcode Attributes

| Name | Value | Meaning |
| --- | --- | --- |
| `BARCODEATTRIB_REVERSE` | `0x0008` | Reverse barcode colors / polarity |
| `BARCODEATTRIB_HUMANREAD` | `0x1000` | Show human-readable text |
| `BARCODEATTRIB_CHECKNUM` | `0x0004` | Use check digit |
| `BARCODEATTRIB_PDF417_SHORTMODE` | `0x0040` | Use PDF417 compact mode |
| `BARCODEATTRIB_DATAMTX_DOTMODE` | `0x0080` | Use DataMatrix dot mode |
| `BARCODEATTRIB_CIRCLEMODE` | `0x0100` | Use circular mode for custom 2D code |

### Font Attribute Flags

| Name | Value | Meaning |
| --- | --- | --- |
| `FONTATB_JSF` | `0x0001` | JczSingle font |
| `FONTATB_TTF` | `0x0002` | TrueType font |
| `FONTATB_DMF` | `0x0004` | DotMatrix font |
| `FONTATB_BCF` | `0x0008` | Barcode font |

### Barcode Types

`BARCODETYPE_39`, `BARCODETYPE_93`, `BARCODETYPE_128A`, `BARCODETYPE_128B`, `BARCODETYPE_128C`, `BARCODETYPE_128OPT`, `BARCODETYPE_EAN128A`, `BARCODETYPE_EAN128B`, `BARCODETYPE_EAN128C`, `BARCODETYPE_EAN13`, `BARCODETYPE_EAN8`, `BARCODETYPE_UPCA`, `BARCODETYPE_UPCE`, `BARCODETYPE_25`, `BARCODETYPE_INTER25`, `BARCODETYPE_CODABAR`, `BARCODETYPE_PDF417`, `BARCODETYPE_DATAMTX`, `BARCODETYPE_USERDEF`

### DataMatrix Size Modes

The header defines `DATAMTX_SIZEMODE_SMALLEST` and fixed size modes from `10x10` through `144x144`, plus rectangular modes such as `8x18`, `8x32`, `12x26`, `12x36`, `16x36`, and `16x48`.

## Structs

### `lmc1_FontRecord`

| Field | Type | Meaning |
| --- | --- | --- |
| `szFontName[256]` | `TCHAR[256]` | Font name |
| `dwFontAttrib` | `DWORD` | Font attribute flags |

## Alignment Map

The alignment parameter used by several APIs follows this index layout:

```text
6 --- 5 --- 4
|           |
|           |
7     8     3
|           |
|           |
0 --- 1 --- 2
```

## API List

### Initialization and Runtime

| API | Signature | Description |
| --- | --- | --- |
| `LMC1_INITIAL` | `int (*)(TCHAR* strEzCadPath, BOOL bTestMode, HWND hOwenWnd)` | Initialize the LMC1 controller |
| `LMC1_CLOSE` | `int (*)()` | Close the LMC1 controller |
| `LMC1_SETDEVCFG` | `int (*)()` | Open the device configuration dialog |
| `LMC1_READPORT` | `int (*)(WORD& data)` | Read the input port value |
| `LMC1_WRITEPORT` | `int (*)(WORD data)` | Write data to the output port |
| `LMC1_LASERON` | `int (*)(BOOL bOpen)` | Turn the laser on or off directly |
| `LMC1_SETPWM` | `int (*)(WORD wPulseHalfPeriod, WORD wPulseWidth)` | Configure PWM output timing |
| `LMC1_SETIPGPOWER` | `int (*)(double dRatio)` | Set IPG laser output power |
| `LMC1_GETCLIENTID` | `WORD (*)()` | Get the hardware dongle client ID |

### Marking and Preview

| API | Signature | Description |
| --- | --- | --- |
| `LMC1_MARK` | `int (*)(BOOL bFlyMark)` | Mark all entities in the current database |
| `LMC1_MARKENTITY` | `int (*)(TCHAR* strEntName)` | Mark one named entity |
| `LMC1_MARKENTITYFLY` | `int (*)(TCHAR* strEntName)` | Fly-mark one named entity |
| `LMC1_REDLIGHTMARK` | `int (*)()` | Mark the red-light preview frame once |
| `LMC1_GETPREVBITMAP` | `CBitmap* (*)(HWND hwnd, int nBMPWIDTH, int nBMPHEIGHT)` | Get a preview bitmap for all entities |
| `LMC1_GETPREVBITMAPBYNAME` | `CBitmap* (*)(TCHAR* strEntName, HWND hwnd, int nBMPWIDTH, int nBMPHEIGHT)` | Get a preview bitmap for one named entity |
| `LMC1_MARKLINE` | `int (*)(double x1, double y1, double x2, double y2, int pen)` | Mark a line using a pen profile |
| `LMC1_MARKLINE2` | `int (*)(double x1, double y1, double x2, double y2, double dSpeed, double dPowerRatio, int nFreq)` | Mark a line using explicit speed, power, and frequency |
| `LMC1_MARKPOINT` | `int (*)(double x, double y, double delay, int pen)` | Mark a point |

### File and Entity Library

| API | Signature | Description |
| --- | --- | --- |
| `LMC1_LOADEZDFILE` | `int (*)(TCHAR* strFileName)` | Load an EZD file and clear the current database |
| `LMC1_SAVEENTLIBTOFILE` | `int (*)(TCHAR* strFileName)` | Save all current entities to an EZD file |
| `LMC1_CLEARENTLIB` | `int (*)()` | Clear the entity library |
| `LMC1_GETENTITYCOUNT` | `int (*)()` | Get the total entity count |
| `LMC1_GETENTITYNAME` | `int (*)(int nEntityIndex, TCHAR szEntName[256])` | Get an entity name by index |
| `LMC1_DELETEENT` | `int (*)(TCHAR* pEntName)` | Delete an entity |
| `LMC1_COPYENT` | `int (*)(TCHAR* strEntName, TCHAR* strNewEntName)` | Copy an entity to a new name |
| `LMC1_CHANGEENTNAME` | `int (*)(TCHAR* strEntName, TCHAR* strNewName)` | Rename an entity |
| `LMC1_GETENTSIZE` | `int (*)(TCHAR* pEntName, double& dMinx, double& dMiny, double& dMaxx, double& dMaxy, double& dZ)` | Get the bounding box of one entity or the whole database |

### Create and Edit Entities

| API | Signature | Description |
| --- | --- | --- |
| `LMC1_ADDTEXTTOLIB` | `int (*)(TCHAR* pStr, TCHAR* pEntName, double dPosX, double dPosY, double dPosZ, int nAlign, double dTextRotateAngle, int nPenNo, BOOL bHatchText)` | Add a text entity |
| `LMC1_ADDFILETOLIB` | `int (*)(TCHAR* pFileName, TCHAR* pEntName, double dPosX, double dPosY, double dPosZ, int nAlign, double dRatio, int nPenNo, BOOL bHatchFile)` | Add a file-based entity |
| `LMC1_ADDCURVETOLIB` | `int (*)(double ptBuf[][2], int ptNum, TCHAR* pEntName, int nPenNo, int bHatch)` | Add a curve entity |
| `LMC1_ADDBARCODETOLIB` | `int (*)(TCHAR* pStr, TCHAR* pEntName, double dPosX, double dPosY, double dPosZ, int nAlign, int nPenNo, int bHatchText, int nBarcodeType, WORD wBarCodeAttrib, double dHeight, double dNarrowWidth, double dBarWidthScale[4], double dSpaceWidthScale[4], double dMidCharSpaceScale, double dQuietLeftScale, double dQuietMidScale, double dQuietRightScale, double dQuietTopScale, double dQuietBottomScale, int nRow, int nCol, int nCheckLevel, int nSizeMode, double dTextHeight, double dTextWidth, double dTextOffsetX, double dTextOffsetY, double dTextSpace, double dDiameter, TCHAR* pTextFontName)` | Add a barcode entity |
| `LMC1_CHANGETEXTBYNAME` | `int (*)(TCHAR* strTextName, TCHAR* strTextNew)` | Change the text content of a text entity |
| `LMC1_GETTEXTBYNAME` | `int (*)(TCHAR* strTextName, TCHAR strText[256])` | Read the text content of a text entity |

### Hatch, Font, and Pen Settings

| API | Signature | Description |
| --- | --- | --- |
| `LMC1_SETHATCHPARAM` | `int (*)(BOOL bEnableContour, int bEnableHatch1, int nPenNo1, int nHatchAttrib1, double dHatchEdgeDist1, double dHatchLineDist1, double dHatchStartOffset1, double dHatchEndOffset1, double dHatchAngle1, int bEnableHatch2, int nPenNo2, int nHatchAttrib2, double dHatchEdgeDist2, double dHatchLineDist2, double dHatchStartOffset2, double dHatchEndOffset2, double dHatchAngle2)` | Set default hatch parameters for newly added entities |
| `LMC1_SETFONTPARAM` | `int (*)(TCHAR* strFontName, double dCharHeight, double dCharWidth, double dCharAngle, double dCharSpace, double dLineSpace, BOOL bEqualCharWidth)` | Set default font parameters for newly added text |
| `LMC1_GETALLFONTRECORD` | `lmc1_FontRecord* (*)(int& nFontNum)` | Get all supported fonts |
| `LMC1_GETPENPARAM` | `int (*)(int nPenNo, int& nMarkLoop, double& dMarkSpeed, double& dPowerRatio, double& dCurrent, int& nFreq, double& dQPulseWidth, int& nStartTC, int& nLaserOffTC, int& nEndTC, int& nPolyTC, double& dJumpSpeed, int& nJumpPosTC, int& nJumpDistTC, double& dEndComp, double& dAccDist, double& dPointTime, BOOL& bPulsePointMode, int& nPulseNum, double& dFlySpeed)` | Get pen processing parameters |
| `LMC1_GETPENPARAM2` | `int (*)(int nPenNo, int& nMarkLoop, double& dMarkSpeed, double& dPowerRatio, double& dCurrent, int& nFreq, double& dQPulseWidth, int& nStartTC, int& nLaserOffTC, int& nEndTC, int& nPolyTC, double& dJumpSpeed, int& nJumpPosTC, int& nJumpDistTC, double& dPointTime, int& nSpiWave, BOOL& bWobbleMode, double& bWobbleDiameter, double& bWobbleDist)` | Get extended pen processing parameters |
| `LMC1_SETPENPARAM` | `int (*)(int nPenNo, int nMarkLoop, double dMarkSpeed, double dPowerRatio, double dCurrent, int nFreq, double dQPulseWidth, int nStartTC, int nLaserOffTC, int nEndTC, int nPolyTC, double dJumpSpeed, int nJumpPosTC, int nJumpDistTC, double dEndComp, double dAccDist, double dPointTime, BOOL bPulsePointMode, int nPulseNum, double dFlySpeed)` | Set pen processing parameters |
| `LMC1_SETPENPARAM2` | `int (*)(int nPenNo, int nMarkLoop, double dMarkSpeed, double dPowerRatio, double dCurrent, int nFreq, double dQPulseWidth, int nStartTC, int nLaserOffTC, int nEndTC, int nPolyTC, double dJumpSpeed, int nJumpPosTC, int nJumpDistTC, int nSpiWave, BOOL bWobbleMode, double bWobbleDiameter, double bWobbleDist)` | Set extended pen processing parameters |

### Geometry and Transform

| API | Signature | Description |
| --- | --- | --- |
| `LMC1_SETROTATEPARAM` | `void (*)(double dCenterX, double dCenterY, double dRotateAng)` | Set rotation transform parameters for subsequent operations |
| `LMC1_MOVEENT` | `int (*)(TCHAR* pEntName, double dMovex, double dMovey)` | Move an entity by a relative offset |
| `LMC1_SCALEENT` | `int (*)(TCHAR* pEntName, double dCenx, double dCeny, double dScaleX, double dScaleY)` | Scale an entity around a center point |
| `LMC1_MIRRORENT` | `int (*)(TCHAR* pEntName, double dCenx, double dCeny, BOOL bMirrorX, BOOL bMirrorY)` | Mirror an entity around a center point |
| `LMC1_ROTATEENT` | `int (*)(TCHAR* pEntName, double dCenx, double dCeny, double dAngle)` | Rotate an entity around a center point |
| `LMC1_GETCURCOOR` | `int (*)(double& x, double& y)` | Get the current galvo position |
| `LMC1_GOTOPOS` | `int (*)(double x, double y)` | Move the galvo directly to a position |

### Extended Axis

| API | Signature | Description |
| --- | --- | --- |
| `LMC1_AXISMOVETO` | `int (*)(int axis, double GoalPos)` | Move extended axis 0 or 1 to a coordinate |
| `LMC1_AXISCORRECTORIGIN` | `int (*)(int axis)` | Correct / reset the origin of an extended axis |
| `LMC1_GETAXISCOOR` | `double (*)(int axis)` | Get the current coordinate of an extended axis |
| `LMC1_AXISMOVETOPULSE` | `int (*)(int axis, int nGoalPos)` | Move extended axis 0 or 1 to a pulse position |
| `LMC1_GETAXISCOORPULSE` | `int (*)(int axis)` | Get the current pulse coordinate of an extended axis |
| `LMC1_RESET` | `double (*)(BOOL bEnAxis0, BOOL bEnAxis1)` | Reset the enabled extended-axis coordinates |

## Notes

- `TCHAR`, `BOOL`, `HWND`, `WORD`, `DWORD`, and `CBitmap` come from the Windows / MFC type system and are not defined in this header.
- The header exposes SDK entry-point types, so practical use usually means loading the DLL and binding these function pointers first.
