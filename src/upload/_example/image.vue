<template>
  <t-space direction="vertical">
    <t-space>
      <t-checkbox v-model="disabled"> 禁用状态 </t-checkbox>
      <t-checkbox v-model="uploadAllFilesInOneRequest"> 多个文件一个请求上传 </t-checkbox>
      <t-checkbox v-model="autoUpload"> 自动上传 </t-checkbox>
      <t-button
        v-if="!autoUpload"
        variant="base"
        theme="default"
        size="small"
        style="height: 22px"
        @click="uploadFiles"
      >
        点击上传
      </t-button>
    </t-space>

    <br />
    <t-space>
      <t-upload
        ref="uploadRef1"
        v-model="file1"
        action="https://service-bv448zsw-1257786608.gz.apigw.tencentcs.com/api/upload-demo"
        theme="image"
        tips="单张图片文件上传（上传成功状态演示）"
        accept="image/*"
        :disabled="disabled"
        :auto-upload="autoUpload"
        :upload-all-files-in-one-request="uploadAllFilesInOneRequest"
        :locale="{
          triggerUploadText: {
            image: '请选择图片',
          },
        }"
        @fail="handleFail"
      ></t-upload>

      <t-upload
        v-model="fileFail"
        action="https://service-bv448zsw-1257786608.gz.apigw.tencentcs.com/api/upload-demo"
        theme="image"
        tips="单张图片文件上传（上传失败状态演示）"
        accept="image/*"
        :format-response="formatResponse"
      ></t-upload>
    </t-space>

    <t-upload
      ref="uploadRef2"
      v-model="file2"
      action="https://service-bv448zsw-1257786608.gz.apigw.tencentcs.com/api/upload-demo"
      theme="image"
      accept="image/*"
      :disabled="disabled"
      :auto-upload="autoUpload"
      :upload-all-files-in-one-request="uploadAllFilesInOneRequest"
      :format-response="formatImgResponse"
      @fail="handleFail"
    ></t-upload>

    <t-upload
      ref="uploadRef3"
      v-model="files"
      action="https://service-bv448zsw-1257786608.gz.apigw.tencentcs.com/api/upload-demo"
      theme="image"
      tips="允许选择多张图片文件上传，最多只能上传 3 张图片"
      accept="image/*"
      :abridge-name="[6, 6]"
      :disabled="disabled"
      :auto-upload="autoUpload"
      :upload-all-files-in-one-request="uploadAllFilesInOneRequest"
      multiple
      :max="3"
      @fail="handleFail"
    ></t-upload>
  </t-space>
</template>
<script setup>
import { ref } from 'vue';
import { MessagePlugin } from 'tdesign-vue-next';

const file1 = ref([]);
const file2 = ref([{ name: 'demo-image-1.png', url: 'https://tdesign.gtimg.com/demo/demo-image-1.png' }]);
const files = ref([]);
const fileFail = ref([]);
const disabled = ref(false);
const uploadAllFilesInOneRequest = ref(false);
const autoUpload = ref(true);

const uploadRef1 = ref();
const uploadRef2 = ref();
const uploadRef3 = ref();

const handleFail = ({ file }) => {
  MessagePlugin.error(`文件 ${file.name} 上传失败`);
};

const formatImgResponse = () => {
  return { name: 'FileName', url: 'https://tdesign.gtimg.com/demo/demo-image-1.png' };
};

const formatResponse = () => {
  return { name: 'FileName', error: '网络异常，图片上传失败' };
};

const uploadFiles = () => {
  uploadRef1.value.uploadFiles();
  uploadRef2.value.uploadFiles();
  uploadRef3.value.uploadFiles();
};
</script>
