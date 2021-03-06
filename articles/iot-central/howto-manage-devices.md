---
title: Azure IoT Central 애플리케이션에서 장치 관리 | Microsoft Docs
description: 운영자로서 Azure IoT Central 애플리케이션에서 장치를 관리하는 방법을 알아봅니다.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: e4746620f083996bf64e77617ec472c3d3894d91
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65464268"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Azure IoT Central 애플리케이션에서 장치 관리

이 문서에서는 운영자로서 Azure IoT Central 애플리케이션에서 디바이스를 관리하는 방법을 설명합니다. 운영자로서 다음과 같은 일을 할 수 있습니다.

- **Device Explorer** 페이지를 사용하여 Azure IoT Central 애플리케이션에 연결될 디바이스를 보고, 추가하며 삭제할 수 있습니다.
- 디바이스 인벤토리를 최신 상태로 유지합니다.
- 디바이스 속성에 저장된 값을 변경하여 디바이스 메타데이터를 최신 상태로 유지합니다.
- **설정** 페이지에서 특정 디바이스의 설정을 업데이트하여 디바이스 동작을 제어합니다.

## <a name="view-your-devices"></a>디바이스 보기

개별 디바이스를 보려면:

1. 왼쪽 탐색 메뉴에서 **Device Explorer**를 선택합니다. 여기서 [디바이스 템플릿](howto-set-up-template.md) 목록을 볼 수 있습니다.

1. **템플릿** 목록에서 디바이스 템플릿을 선택합니다.

1. **Device Explorer** 페이지의 오른쪽 창에 해당 디바이스 템플릿으로 만든 디바이스 목록이 표시됩니다. 개별 디바이스를 선택하면 해당 디바이스의 디바이스 세부 정보 페이지를 볼 수 있습니다.

    ![디바이스 세부 정보 페이지](./media/howto-manage-devices/devicelist.png)

## <a name="add-a-device"></a>디바이스 추가

Azure IoT Central 애플리케이션에 장치를 추가하려면:

1. 왼쪽 탐색 메뉴에서 **Device Explorer**를 선택합니다.

1. 디바이스를 만들 디바이스 템플릿을 선택합니다.

1. + **새로 만들기**를 선택합니다.

1. **실제 장치** 또는 **시뮬레이션된 장치**를 선택합니다. 실제 장치는 Azure IoT Central 애플리케이션에 연결하는 물리적 장치입니다. 시뮬레이션된 디바이스에는 Azure IoT Central에서 생성한 샘플 데이터가 있습니다.

## <a name="import-devices"></a>디바이스 가져오기

대량의 디바이스를 애플리케이션에 연결하려면 CSV 파일에서 대량으로 디바이스를 가져오면 됩니다. CSV 파일에는 다음 열과 헤더가 있습니다.

* **IOTC_DeviceID** - 디바이스 ID는 모두 소문자여야 합니다.
* **IOTC_DeviceName** - 이 열은 선택 사항입니다.

장치를 애플리케이션에 대량으로 등록하려면:

1. 왼쪽 탐색 메뉴에서 **Device Explorer**를 선택합니다.

1. 왼쪽 패널에서 디바이스를 대량으로 만들 디바이스 템플릿을 선택합니다.

    > [!NOTE]
    > 아직 디바이스 템플릿이 없는 경우 템플릿 없이 **연결되지 않은 디바이스** 아래에서 디바이스를 가져와서 등록할 수 있습니다. 디바이스를 가져온 후 디바이스를 템플릿에 연결할 수 있습니다.

1. **가져오기**를 선택합니다.

    ![가져오기 작업](./media/howto-manage-devices/bulkimport1a.png)

1. 가져올 디바이스 ID 목록이 들어 있는 CSV 파일을 선택합니다.

1. 파일이 업로드되면 디바이스 가져오기가 시작됩니다. 디바이스 그리드 위쪽에서 가져오기 상태를 추적할 수 있습니다.

1. 가져오기가 완료되면 디바이스 그리드에 성공 메시지가 표시됩니다.

    ![가져오기 성공](./media/howto-manage-devices/bulkimport3a.png)

디바이스 가져오기 작업이 실패하면 디바이스 그리드에 오류 메시지가 표시됩니다. 모든 오류를 캡처하는 로그 파일이 생성되며 이 파일을 다운로드할 수 있습니다.

**템플릿과 디바이스 연결**

**연결되지 않은 디바이스** 아래에서 가져오기를 시작하여 디바이스를 등록하는 경우 해당 디바이스는 어떤 디바이스 템플릿 연결 없이도 생성됩니다. 디바이스에 대한 데이터 및 기타 세부 정보를 살펴보려면 디바이스가 템플릿과 연결되어 있어야 합니다. 템플릿과 디바이스를 연결하려면 이러한 단계를 수행합니다.

1. 왼쪽 탐색 메뉴에서 **Device Explorer**를 선택합니다.

1. 왼쪽 패널에서 **연결되지 않은 디바이스**를 선택합니다.

    ![연결되지 않은 디바이스](./media/howto-manage-devices/unassociateddevices1a.png)

1. 템플릿과 연결하려는 디바이스를 선택합니다.

1. 선택 **연결**:

    ![디바이스 연결](./media/howto-manage-devices/unassociateddevices2a.png)

1. 사용 가능한 템플릿 목록에서 템플릿을 선택 하 고 선택 **연결**합니다.

1. 선택한 디바이스가 선택한 디바이스 템플릿과 연결됩니다.

> [!NOTE]
> 디바이스가 템플릿과 연결된 후에는 연결을 해제하거나 다른 템플릿과 연결할 수 없습니다.

## <a name="export-devices"></a>내보내기 디바이스

실제 디바이스를 IoT Central에 연결하려면 해당 연결 문자열이 필요합니다. 장치 연결 문자열을 생성 해야 하는 정보를 가져오는 대량 장치 세부 정보를 내보낼 수 있습니다. 내보내기 프로세스는 장치 id, 장치 이름 및 선택한 모든 장치에 대 한 키를 사용 하 여 CSV 파일을 만듭니다.

애플리케이션에서 장치를 대량으로 내보내려면 다음을 수행합니다.

1. 왼쪽 탐색 메뉴에서 **Device Explorer**를 선택합니다.

1. 왼쪽 패널에서 디바이스를 내보낼 디바이스 템플릿을 선택합니다.

1. 내보내기 및 선택 하려는 장치를 선택 합니다 **내보내기** 작업 합니다.

    ![내보내기](./media/howto-manage-devices/export1a.png)

1. 내보내기 프로세스가 시작됩니다. 그리드 위쪽에서 상태를 추적할 수 있습니다.

1. 내보내기가 완료되면 생성된 파일을 다운로드할 수 있는 링크와 함께 성공 메시지가 표시됩니다.

1. 선택 된 **성공 메시지** 디스크의 로컬 폴더에 파일을 다운로드 하려면.

    ![내보내기 성공](./media/howto-manage-devices/export2a.png)

1. 내보낸 CSV 파일에는 디바이스 ID, 디바이스 이름, 디바이스 키 및 X509 인증서 지문 열이 포함되어 있습니다.

    * IOTC_DEVICEID
    * IOTC_DEVICENAME
    * IOTC_SASKEY_PRIMARY
    * IOTC_SASKEY_SECONDARY
    * IOTC_X509THUMBPRINT_PRIMARY
    * IOTC_X509THUMBPRINT_SECONDARY

참조 [Azure IoT Central의 장치 연결](concepts-connectivity.md), 연결 문자열 및 IoT Central 응용 프로그램에 실제 장치를 연결 하는 방법에 대 한 자세한 내용은 합니다.

## <a name="delete-a-device"></a>디바이스 삭제

Azure IoT Central 애플리케이션에서 실제 장치 또는 시뮬레이션된 장치를 삭제하려면:

1. 탐색 메뉴에서 **Device Explorer**를 선택합니다.

1. 삭제할 디바이스의 디바이스 템플릿을 선택합니다.

1. 삭제할 디바이스 옆에 있는 확인란을 선택합니다.

1. **삭제**를 선택합니다.

## <a name="change-a-device-setting"></a>디바이스 설정 변경

설정은 디바이스의 동작을 제어합니다. 다시 말해서, 설정을 통해 디바이스에 입력을 제공할 수 있습니다. **디바이스 세부 정보** 페이지에서 디바이스 설정을 살펴보고 업데이트할 수 있습니다.

1. 탐색 메뉴에서 **Device Explorer**를 선택합니다.

1. 설정을 변경하려는 디바이스의 디바이스 템플릿을 선택합니다.

1. **설정** 탭을 선택합니다. 디바이스에 있는 모든 설정과 현재 값이 표시됩니다. 각 설정에 대해 디바이스가 여전히 동기화 중인지 확인할 수 있습니다.

1. 설정을 필요한 값으로 수정합니다. 한 번에 여러 설정을 수정하고 동시에 업데이트할 수 있습니다.

1. **업데이트**를 선택합니다. 값이 디바이스로 전송됩니다. 디바이스에서 설정 변경을 확인하면 설정 상태가 **동기화됨**으로 돌아갑니다.

## <a name="change-a-property"></a>속성 변경

속성은 도시나 일련 번호처럼 디바이스와 연결된 디바이스 메타데이터입니다. **디바이스 세부 정보** 페이지에서 속성을 살펴보고 업데이트할 수 있습니다.

1. 탐색 메뉴에서 **Device Explorer**를 선택합니다.

1. 속성을 변경하려는 디바이스의 디바이스 템플릿을 선택합니다.

1. **속성** 탭을 선택하면 모든 속성이 표시됩니다.

1. 애플리케이션 속성을 원하는 값으로 수정합니다. 한 번에 여러 속성을 수정하고 동시에 업데이트할 수 있습니다. **업데이트**를 선택합니다.

> [!NOTE]
> _디바이스 속성_의 값은 변경할 수 없습니다 장치 속성은 장치에서 설정하며 Azure IoT Central 애플리케이션 내에서 읽기 전용입니다.

## <a name="next-steps"></a>다음 단계

Azure IoT Central 애플리케이션에서 디바이스를 관리하는 방법을 알아보았으니, 다음과 같은 후속 단계를 진행하시기 바랍니다.

> [!div class="nextstepaction"]
> [디바이스 집합을 사용하는 방법](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->
