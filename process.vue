<template>
    <div class="process">
        <div class="process_box">
            <el-row
                v-for="(row, index) in process"
                :key="index" class="process_list"
                :ref="(row.num != 1 ? 'ref_' + getRandom()  : null)"
            >
                <div
                    v-if="(row.num != 1)"
                    :class="[(row.num != 1 ? 'row_line' : ''), row.unset_border]"
                >
                </div>
                <div
                    v-for="(item, key) in row.list"
                    :key="key" :style="{width: item.width}"
                    :class="[
                        item.class_name,
                        'box_li',
                        (item.clear == 'before' ? 'clear_before' : (item.clear == 'after' ? 'clear_after' : ''))
                        ]"
                >
                    {{item.label}}
                </div>
            </el-row>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Process",
        data() {
            return {
                process: [
                    {
                        num: 1, // 区块个数
                        list: [
                            {
                                class_name: 'blue', // 区块背景及文字颜色
                                label: '应用名称', // 区块名称
                                width: '8%', // 区块宽度
                                clear: 'before' // 是否清楚伪类 伪类类型：before/after
                            }
                        ]
                    },
                    {
                        num: 1,
                        list: [
                            {
                                class_name: 'blue',
                                label: '1.服务器、域名',
                                width: '8%',
                                clear: 'before',
                                id: 'point-server',
                            }
                        ]
                    },
                    {
                        num: 1,
                        list: [
                            {
                                class_name: 'blue',
                                label: '2.图片存储',
                                width: '8%',
                                clear: 'before',
                                id: 'point-file'
                            }
                        ]
                    },
                    {
                        num: 2,
                        unset_border: 'unset_bottom',
                        list: [
                            {
                                class_name: 'blue',
                                label: '3.提供包名',
                                width: '8%',
                                id: 'point-package'
                            },
                            {
                                class_name: 'blue',
                                label: '4.制作Logo',
                                width: '8%',
                                id: 'point-logo'
                            }
                        ]
                    },
                    {
                        num: 7,
                        list: [
                            {
                                class_name: 'blue',
                                label: '5.苹果开发者账号',
                                width: '8%',
                                id: 'point-ios'
                            },
                            {
                                class_name: 'blue',
                                label: '6.谷歌登录、支付',
                                width: '8%',
                                id: 'point-google'
                            },
                            {
                                class_name: 'blue',
                                label: '7.Fb登录、支付',
                                width: '8%',
                                id: 'point-facebook'
                            },
                            {
                                class_name: 'blue',
                                label: '8.腾讯企业邮箱',
                                width: '8%',
                                id: 'point-email'
                            },
                            {
                                class_name: 'blue',
                                label: '9.推送',
                                width: '8%',
                                id: 'point-push'
                            },
                            {
                                class_name: 'blue',
                                label: '10.启动图',
                                width: '8%',
                                id: 'point-start-photo'
                            },
                            {
                                class_name: 'blue',
                                label: '11.苹果税务信息',
                                width: '8%',
                                id: 'point-tax'
                            },
                        ]
                    },
                    {
                        num: 1,
                        list: [
                            {
                                class_name: 'blue',
                                label: 'APP资料已补齐',
                                width: '8%',
                                clear: 'after'
                            }
                        ]
                    },
                    {
                        num: 1,
                        list: [
                            {
                                class_name: 'blue',
                                label: '测试',
                                width: '8%',
                                clear: 'after'
                            }
                        ]
                    },
                    {
                        num: 1,
                        list: [
                            {
                                class_name: 'blue',
                                label: '上线',
                                width: '8%',
                                clear: 'after',
                            }
                        ]
                    }
                ]
            }
        },
        mounted() {
            let _this = this;
            this.$nextTick(() => {
                for (let ref in _this.$refs) {

                    if (_this.$refs[ref][0].$el.children == undefined) {
                        continue;
                    }

                    let _children = _this.$refs[ref][0].$el.children;
                    let widthArr  = [];
                    let leftArr   = [];
                    let lineObj;
                    let width;
                    let left;

                    for (let child in _children) {
                        if (typeof _children[child] != 'object') {
                            continue;
                        }

                        if (_children[child].className.indexOf('row_line') != -1) {
                            lineObj = _children[child];
                            continue;
                        }

                        widthArr.push(_children[child].clientWidth);
                        leftArr.push(_children[child].offsetLeft);
                    }

                    let firstWidth = widthArr.shift();
                    let endWidth   = widthArr.pop();
                    let firstLeft  = leftArr.shift();
                    let endLeft    = leftArr.pop();

                    width = (lineObj.clientWidth -((firstWidth + endWidth) / 2 + (lineObj.clientWidth - endLeft - endWidth) + firstLeft)) / lineObj.clientWidth;
                    left  = (firstLeft + firstWidth / 2 + 1) / lineObj.clientWidth;

                    lineObj.style.width = width * 100 + '%';
                    lineObj.style.left  = left * 100 + '%';
                }
            });
        },
        methods: {
            getRandom() { // 随机生成6位数，保持ref的唯一性
                let number = parseInt(Math.random() * 1000000);
                return number;
            },
            jumpId(id) {
                this.$emit('jump', id)
            }
        }
    }
</script>

<style scoped>
    .process {
        background-color: #ffffff;
        box-shadow: 1px 1px 5px #F4F5F9;
        padding: 30px;
        margin: 30px;
    }
    .process_box {
        width: 100%;
        height: auto;
    }
    .process_list {
        width: 100%;
        display: flex;
        justify-content: space-between;
    }
    .box_li {
        height: 30px;
        line-height: 30px;
        font-size: 12px;
        border-radius: 3px;
        color: #999999;
        border: 1px solid #999999;
        cursor: pointer;
        text-align: center;
        text-decoration: none;
        position: relative;
        margin: 30px 0;
    }
    .row_line {
        width: 100%;
        height: 90px;
        position: absolute;
        border-top: 1px solid #b5b5b5;
        border-bottom: 1px solid #b5b5b5;
    }
    .unset_bottom {
        border-bottom: 0 !important;
    }
    .clear_before {
        margin-top: 0 !important;
    }
    .clear_before:before {
        content: '';
        height: 0 !important;
        background-color: unset !important;
    }
    .clear_after {
        margin-bottom: 0 !important;
    }
    .clear_after:after {
        content: '';
        height: 0 !important;
        background-color: unset !important;
    }
    .box_li:after {
        content: '';
        display: block;
        height: 30px;
        width: 0.1px;
        position: absolute;
        bottom: -31px;
        background-color: #b5b5b5;
        left: 50%;
        transform: translateX(-50%);
        -webkit-transform: translateX(-50%);
    }
    .box_li:before {
        content: '';
        display: block;
        height: 30px;
        width: 0.1px;
        position: absolute;
        top: -31px;
        background-color: #b5b5b5;
        left: 50%;
        transform: translateX(-50%);
        -webkit-transform: translateX(-50%);
    }
    .blue {
        color: #fff!important;
        background-color: #00a3ff !important;
        border: 1px solid #00a3ff!important;
    }
    .grey {
        background-color: #f2f2f2 !important;
        border: 1px solid #f2f2f2 !important;
    }
</style>
