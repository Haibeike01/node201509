import React, { Component } from "react";
import {
  StyleSheet,
  DeviceEventEmitter,
} from "react-native";
import FormVerify from "../../js/formVerify";
import { Dialog, GlobalDataUtils, Ajax, Attachment } from '../../component/baseComponent';
import BuildingFromWidget from './formInfo';

class FormInput extends Component {
  constructor(props) {
    super(props);
    let loginUserRegion = GlobalDataUtils.getLoginUserRegion();
    this.state = {
      formData: {
        "regionNo": loginUserRegion.regionNo,
        "regionName": loginUserRegion.regionName,
      }
    };
  }

  // 获取表单的值
  async formValue(page, refs, data) {
    // // 获取到两个值，然后进行效验
    if (FormVerify.getVerify(refs, data)) {
      data.backgroundColor = '#7854f1';
      data.borderColor = '#9b84e7';
      let geom = await refs.geom.getPolygon();
      if(!!geom){
        data.geom = geom;
      }
      if (!data.attachments || data.attachments == '') {
        data.attachments = [];
      }
      let url = "/api/building/save";
      if (data.attachments != null && data.attachments.length > 0) {
        Attachment.uploads(page, data.attachments, {
          finish: function (attachments) {
            data.attachments = attachments;
            Ajax.get().postJson(url, data, {
              success: function (data) {
                Dialog.success("提交成功", {
                  close: function () {
                    DeviceEventEmitter.emit("reload_building_list");
                    Actions.pop();
                  }
                });
              }
            });
          }
        });
      } else {
        Ajax.get().postJson(url, data, {
          success: function (data) {
            Dialog.success("提交成功", {
              close: function () {
                DeviceEventEmitter.emit("reload_building_list");
                Actions.pop();
              }
            });
          }
        });
      }
    }
  }

  render() {
    const { formData } = this.state;
    return (
      <BuildingFromWidget formData={formData} submit={(page, refsData, fmdata) => { this.formValue(page, refsData, fmdata) }} btntitle={"保存"} />
    );
  }
}

export default class buildingAddScreens extends React.Component {
  static navigationOptions = {
    headerTitle: "新增楼栋"
  };
  render() {
    return (
      <FormInput />
    );
  }
}

const styles = StyleSheet.create({

});
