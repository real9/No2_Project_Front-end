<template>
<!--  原来的FormView，现在是数据端口-->
<div>
    <nav-bar></nav-bar>
    <div id="form-view" class="formView">
        <div id="side">
            <side-bar></side-bar>
        </div>
        <div id="form">
            <el-collapse v-model="activeFormGroup" accordion @change="changeCoItem">
                <el-collapse-item title="一、基本信息" name="formGroup1">
                    <el-form ref="form1" id="form1" :model="form1" @click.native="onForm" label-width="300px">
                        <el-form-item label="主刀医师">
                            <el-input v-model="form1.HB1_1" name="1"></el-input>
                        </el-form-item>
                        <el-form-item label="质控医师（科主任）">
                            <el-input v-model="form1.HB1_2" name="2"></el-input>
                        </el-form-item>
                        <el-form-item label="质控护士（护士长）">
                            <el-input v-model="form1.HB1_3" name="3"></el-input>
                        </el-form-item>
                        <el-form-item label="主控医师（管床医生）">
                            <el-input v-model="form1.HB1_4" name="4"></el-input>
                        </el-form-item>
                        <el-form-item label="责任护士（管床护士）">
                            <el-input v-model="form1.HB1_5" name="5"></el-input>
                        </el-form-item>
                        <el-form-item label="上报科室">
                            <el-input v-model="form1.HB1_6" name="6"></el-input>
                        </el-form-item>
                        <el-form-item label="患者住院号">
                            <el-input v-model="form1.HB1_7" name="7"></el-input>
                        </el-form-item>
                        <el-form-item label="患者身份证号">
                            <el-input v-model="form1.HB1_8" name="8"></el-input>
                        </el-form-item>
                        <el-form-item label="出生日期">
                            <el-input v-model="form1.HB1_9" :disabled="true"></el-input>
                        </el-form-item>
                        <el-form-item label="患者性别">
                            <el-input v-model="form1.HB1_10" :disabled="true"></el-input>
                        </el-form-item>
                        <el-form-item label="患者体重（kg）">
                            <el-input v-model="form1.HB1_11" name="11"></el-input>
                        </el-form-item>
                        <el-form-item label="患者身高（cm）">
                            <el-input v-model="form1.HB1_12" name="12"></el-input>
                        </el-form-item>
                        <el-form-item label="体重指数（kg/m2）">
                            <el-input v-model="form1.HB1_13" name="13" :disabled="true">
                            </el-input>
                        </el-form-item>
                        <el-form-item label="主要诊断ICD-10四位亚目编码与名称">
                            <el-select v-model="form1.HB1_14" name="14" @visible-change="onForm" placeholder="请选择">
                                <el-option v-for="item in option1_14" :key="item.value" :label="item.label"
                                    :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="主要诊断ICD-10六位临床扩展编码与名称">
                            <el-select v-model="form1.HB1_15" name="15" @visible-change="onForm" placeholder="请选择">
                                <el-option v-for="item in option1_15[form1.HB1_14.charCodeAt()-'a'.charCodeAt()]"
                                    :key="item.value" :label="item.label" :value="item.value"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="是否出院后31天内重复住院">
                            <el-radio-group v-model="form1.HB1_16" name="16" placeholder="请选择">
                                <el-radio v-for="item in option1_16" :key="item.value" :label="item.value">{{item.label}}
                                </el-radio>
                            </el-radio-group>
                        </el-form-item>
                        <el-form-item label="费用支付方式">
                            <el-select v-model="form1.HB1_17" name="17" @visible-change="onForm" placeholder="请选择">
                                <el-option v-for="item in option1_17" :key="item.value" :label="item.label"
                                    :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="入院时间">
                            <el-date-picker v-model="form1.HB1_18" name="18" type="date" placeholder="请选择日期"></el-date-picker>
                        </el-form-item>
                        <el-form-item label="出院时间">
                            <el-date-picker v-model="form1.HB1_19" name="19" type="date" placeholder="请选择日期"></el-date-picker>
                        </el-form-item>
                        <el-form-item label="此次入院目的">
                            <el-select v-model="form1.HB1_20" name="20" @visible-change="onForm" placeholder="请选择">
                                <el-option v-for="item in option1_20" :key="item.value" :label="item.label"
                                    :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="submit('formGroup1')">保存</el-button>
                            <el-button>重置</el-button>
                        </el-form-item>
                    </el-form>
                </el-collapse-item>
                <keep-alive include="ZongHe,ShouShu,NeiJing">
                    <router-view  @getSuccess='submit' @athing='upAthing' @dthing='upAthing' @onForm='onForm'></router-view>
                </keep-alive>
            </el-collapse>
        </div>
        <div id="progress" >
            <progress-bar :firstActive="firstActive" 
            :secondActive="secondActive" 
            :stageTree.sync="stageTree"
            @back="backSomeWhere"
            ></progress-bar>
        </div>        
    </div>
</div>
</template>

<script>
    import NavBar from '../bars/NavBar.vue'
    import SideBar from '../bars/SideBar.vue'
    import ProgressBar from '../bars/ProgressBar.vue'
    export default {
        name: 'formView',
        components: {
            NavBar,
            SideBar,
            ProgressBar
        },
        data: function () {
            return {
                activeFormGroup: '',
                collapseNumbers : '', 
                stageTree : [],
                firstActive :0,
                secondActive: 0,           
                form1: {
                    HB1_1: '',
                    HB1_2: '',
                    HB1_3: '',
                    HB1_4: '',
                    HB1_5: '',
                    HB1_6: '',
                    HB1_7: '',
                    HB1_8: '',
                    HB1_9: '',
                    HB1_10: '',
                    HB1_11: '',
                    HB1_12: '',
                    HB1_13: '',
                    HB1_14: '',
                    HB1_15: '',
                    HB1_16: '',
                    HB1_17: '',
                    HB1_18: '',
                    HB1_19: '',
                    HB1_20: '',
                },
                option1_14: [{
                        value: 'a',
                        label: ' 贲门恶性肿瘤'
                    },
                    {
                        value: 'b',
                        label: ' 胃底恶性肿瘤'
                    },
                    {
                        value: 'c',
                        label: ' 胃体恶性肿瘤'
                    },
                    {
                        value: 'd',
                        label: ' 幽门窦恶性肿瘤'
                    },
                    {
                        value: 'e',
                        label: ' 幽门恶性肿瘤'
                    },
                    {
                        value: 'f',
                        label: ' 未特指的胃小弯恶性肿瘤'
                    },
                    {
                        value: 'g',
                        label: ' 未特指的胃大弯恶性肿瘤'
                    },
                    {
                        value: 'h',
                        label: ' 胃交搭跨越恶性肿瘤的损害'
                    },
                    {
                        value: 'i',
                        label: ' 未特指的胃恶性肿瘤'
                    },
                ],
                option1_15: [
                    [{
                            value: 'a',
                            label: ' 无'
                        },
                        {
                            value: 'b',
                            label: ' 贲门恶性肿瘤'
                        },
                        {
                            value: 'c',
                            label: ' 贲门口恶性肿瘤'
                        },
                        {
                            value: 'd',
                            label: ' 食管贲门连接处恶性肿瘤'
                        },
                        {
                            value: 'e',
                            label: ' 食管胃连接处恶性肿瘤'
                        }
                    ],
                    [{
                            value: 'f',
                            label: ' 无'
                        },
                        {
                            value: 'g',
                            label: ' 胃底恶性肿瘤'
                        }
                    ],
                    [{
                            value: 'h',
                            label: ' 无'
                        },
                        {
                            value: 'i',
                            label: ' 胃体恶性肿瘤'
                        }
                    ],
                    [{
                            value: 'j',
                            label: ' 无'
                        },
                        {
                            value: 'k',
                            label: ' 幽门窦恶性肿瘤'
                        },
                        {
                            value: 'l',
                            label: ' 胃窦恶性肿瘤'
                        }
                    ],
                    [{
                            value: 'm',
                            label: ' 无'
                        },
                        {
                            value: 'n',
                            label: ' 幽门恶性肿瘤'
                        },
                        {
                            value: 'o',
                            label: ' 幽门前恶性肿瘤'
                        },
                        {
                            value: 'p',
                            label: ' 幽门管恶性肿瘤'
                        }
                    ],
                    [{
                            value: 'q',
                            label: ' 无'
                        },
                        {
                            value: 'r',
                            label: ' 胃小弯恶性肿瘤'
                        }
                    ],
                    [{
                            value: 's',
                            label: ' 无'
                        },
                        {
                            value: 't',
                            label: ' 胃大弯恶性肿瘤'
                        }
                    ],
                    [{
                            value: 'u',
                            label: ' 无'
                        },
                        {
                            value: 'v',
                            label: ' 胃交搭跨越恶性肿瘤'
                        },
                        {
                            value: 'w',
                            label: ' 胃体和胃窦及胃大弯恶性肿瘤'
                        },
                        {
                            value: 'x',
                            label: ' 贲门胃底恶性肿瘤'
                        },
                        {
                            value: 'y',
                            label: ' 贲门胃体恶性肿瘤'
                        },
                        {
                            value: 'z',
                            label: ' 胃窦胃体恶性肿瘤'
                        },
                        {
                            value: 'A',
                            label: ' 胃底胃体恶性肿瘤'
                        }
                    ],
                    [{
                            value: 'B',
                            label: ' 无'
                        },
                        {
                            value: 'C',
                            label: ' 胃恶性肿瘤'
                        },
                        {
                            value: 'D',
                            label: ' 胃多处恶性肿瘤'
                        },
                        {
                            value: 'E',
                            label: ' 胃溃疡癌变'
                        },
                        {
                            value: 'F',
                            label: ' 残胃恶性肿瘤'
                        }
                    ],
                ],
                option1_16: [{
                        value: 'a',
                        label: ' 是'
                    },
                    {
                        value: 'b',
                        label: ' 否'
                    },
                ],
                option1_17: [{
                        value: 'a',
                        label: ' 城镇职工基本医疗保险'
                    },
                    {
                        value: 'b',
                        label: ' 城镇居民基本医疗保险'
                    },
                    {
                        value: 'c',
                        label: ' 新型农村合作医疗'
                    },
                    {
                        value: 'd',
                        label: ' 贫困救助'
                    },
                    {
                        value: 'e',
                        label: ' 商业医疗保险'
                    },
                    {
                        value: 'f',
                        label: ' 公费医疗'
                    },
                    {
                        value: 'g',
                        label: ' 全自费'
                    },
                    {
                        value: 'h',
                        label: ' 其他社会保险（生育保险、工商保险、农民工保险等）'
                    },
                    {
                        value: 'i',
                        label: ' 其他'
                    },
                ],
                option1_20: [{
                        value: 'a',
                        label: ' 手术'
                    },
                    {
                        value: 'b',
                        label: ' 内镜下治疗'
                    },
                    {
                        value: 'c',
                        label: ' 综合治疗'
                    },
                ],
            }
        },
        activated(){
             this.upAthing();
        },   
        methods: {
            submit() {
                let _this = this;
                console.log(_this.form1);
                if (_this.form1.HB1_16 === 'a') {
                    alert("出院后31天内重复住院,不符合上报要求");
                } else {
                    //使用子路由，让该页面首页不变
                    if (_this.form1.HB1_20 === 'a') {
                        _this.$router.replace('/form/shou-shu');
                        _this.activeFormGroup = '';
                    } else if (_this.form1.HB1_20 === 'b') {
                        _this.$router.replace('/form/nei-jing');
                        _this.activeFormGroup = '';
                    } else if (_this.form1.HB1_20 === 'c') {
                        _this.$router.replace('/form/zong-he');
                        _this.activeFormGroup = '';
                    }
                }
            },
            setBirthDateAndSexual() {
                let _this = this;
                let reg = /(^\d{17}(x|X|\d)$)/
                let idCard = _this.form1.HB1_8;
                if (!reg.test(idCard)) {
                    _this.form1.HB1_9 = '请输入正确的第二代身份证号码';
                    _this.form1.HB1_10 = '请输入正确的第二代身份证号码';
                    return false;
                } else {
                    _this.form1.HB1_9 = idCard.slice(6, 14).replace(/(.{4})(.{2})/, "$1-$2-");
                    if (parseInt(idCard.slice(-2, -1)) % 2 == 1) {
                        _this.form1.HB1_10 = '男';
                    } else {
                        _this.form1.HB1_10 = '女';
                    }
                    return true;
                }
            },
            setBmi() {
                let _this = this;
                if (_this.form1.HB1_11 == '' || _this.form1.HB1_12 == '' || _this.form1.HB1_11 == 0 || _this.form1
                    .HB1_12 == 0) {
                    _this.form1.HB1_13 = '';
                    return false;
                }
                let reg = /^\d+(\.\d+)?$/;
                if (reg.test(_this.form1.HB1_11) && reg.test(_this.form1.HB1_12)) {
                    _this.form1.HB1_13 = (parseFloat(_this.form1.HB1_11) / ((parseFloat(_this.form1.HB1_12) / 100) *
                        (
                            parseFloat(_this.form1.HB1_12) / 100))).toFixed(2);
                    return true;
                } else {
                    _this.form1.HB1_13 = '';
                    return false;
                }
            },
            upAthing() {
                let _this = this;
                //计算组件渲染、销毁后的el-collapse-item的个数（在<keep-alive>下）
                let numbers = document.getElementsByClassName('el-collapse-item').length;
                _this.collapseNumbers=numbers;
            },
            changeCoItem(newVal) {
                let _this=this,index=0;
                index=Number(newVal.substr(9));
                _this.stageTree[index-1].progressFlag = true;
                _this.firstActive = index;
            },
            onForm(){
                if(document.activeElement.tagName==='INPUT'){
                    this.secondActive=Number(document.activeElement.getAttribute('name'));
                }
            },
            changeSide(index) {
                let _this = this,
                    activeStatus = _this.PBtitles[index].status;
                if (activeStatus == 'progress') {
                    _this.PBtitles[index].status = 'finish'
                }
            },
            changeCoNumber(val) {
                    var steps = [],step = {},i,form;
                    if(val===1){
                        step.label = 1;
                        step.progressFlag = false;
                        step.secondValue=0;
                        form='form'+1;
                        step.children= this.countChildren(document.getElementById(form))-1;
                        steps.push(step);
                        step = {}                       
                    }else{
                        for (i =1;i <= val;i++){
                            step.label = i;
                            step.progressFlag = false;
                            step.secondValue=0;
                            form='form'+i;
                            step.children= this.countChildren(document.getElementById(form))-1;
                            steps.push(step);
                            step = {}
                    }
                    }
                    this.stageTree=steps;
            },
            countChildren(obj){
                var children=obj.children,childrenItemNum=0;
                for(var i =0; i<children.length;i++){
                    if(children[i].getAttribute("class")==='el-form-item'){
                        childrenItemNum ++
                    }
                }
                return childrenItemNum;
            },
            backSomeWhere(val){
                this.activeFormGroup='formGroup'+val;
                this.changeCoItem(this.activeFormGroup);
            }
        },
        computed: {
            idCardWatch() {
                let _this = this;
                return _this.form1.HB1_8;
            },
            bmiWatch() {
                let _this = this;
                let _arr = [];
                _arr.push(_this.form1.HB1_11);
                _arr.push(_this.form1.HB1_12);
                return _arr.join(",")
            }
        },
        watch: {
            idCardWatch() {
                let _this = this;
                _this.setBirthDateAndSexual();
            },
            bmiWatch() {
                let _this = this;
                _this.setBmi();
            },
            //折叠面板项的个数变化则，，，
            collapseNumbers: function(newCoNumber){
                this.changeCoNumber(newCoNumber);
            }
        }
    }
</script>

<style scoped>
    #form-view {
        display: flex;
        justify-content: flex-start;
    }

    #side {
        width: 15%;
        height: 100%;
        margin: 0px;
    }

    #form {
        width: 75%;
        height: 100%;
        margin: 0px;
    }

    #progress{
        position: fixed;
        top:inherit;
        right:0;
        width: 10%;
        height: 100%;     
    }

    .el-collapse {
        width: 80%;
        margin: 0 10%
    }

    .el-form {
        margin-top: 4%;
        margin-right: 20%
    }

    .formView>>>.el-checkbox__label {
        display: inline-grid;
        white-space: pre-line;
        word-wrap: break-word;
        overflow: hidden;
        line-height: 20px;
    }
</style>