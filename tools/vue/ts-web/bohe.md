```
import '@/assets/less/bohe_2.0.less';
import HeadNav from './components/HeadNav.vue';
import FootNav from './components/FootNav.vue';
import DownloadBox from './components/DownloadBox.vue';
import {Component, Vue} from 'vue-property-decorator';
import {LanguageConfig} from "@/ts/utils/Language";
import GlobalConfig from "@/pages/bohe/global.config";
// import "babel-polyfill";
import WebParamModel from "@/ts/models/WebModel";
import NewsConfigModel, {ActivityRequestModel, NewModel, NewRequestModel} from '@/ts/models/NewsModel';
import HttpClient from '@/ts/net/HttpClient';
import {IdataModel} from '@/ts/models/IdataModel';
import JumpWebUtil from '@/ts/utils/JumpWebUtil';
import LocalStorageUtil from '@/ts/utils/LocalStorageUtil';
import {UserToken} from '@/ts/models/UserModel';
import ProjectConfig from "../../../project.config";
import VueI18n from "vue-i18n";
import {Tooltip} from "element-ui";
import {TdappModel} from '@/ts/models/TdappModel';
import ConfigUtil from "@/ts/utils/ConfigUtil";
import Util from "@/ts/utils/Util";
import $ from 'jquery'
//判断是否跳转移动站点
const mUrl = GlobalConfig.getMobWebBaseUrl() + '/' + 'applinks.html';
JumpWebUtil.checkMobile(mUrl);

Vue.config.productionTip = false;
Vue.use(Tooltip);
//语言包
Vue.use(VueI18n);
const webParam = WebParamModel.getInstace();
let lang = LanguageConfig.getInstance();
lang.init();
const i18n = new VueI18n(lang);

@Component({
    components: {
        'head-nav': HeadNav,
        'foot-nav': FootNav,
        'download-box': DownloadBox,
    }
})
class Index extends Vue {

    public webParam = WebParamModel.getInstace(); // 浏览器参数
    public newsList: Array<NewModel> = [];
    public notifyList: Array<NewModel> = [];
    public activityList: Array<ActivityRequestModel> = [];
    public imageHeadUrl: string = '';
    public token: UserToken = new UserToken();
    public windowsDownloadUrl: string = '';
    public windowsDownloadUrl_old: string = '';
    public macDownloadUrl: string = '';
    public checkBtn: number = 0;
    public browserTipShow: boolean = false;

    //////////公共参数
    public http = new HttpClient();
    public backData: IdataModel<any> | undefined;

    //////////END

    public created() {
        Util.googleSatics(this.webParam.region_code);
        this.debugInfo(); //debug信息
        this.setBaseUrl(GlobalConfig.getBaseUrl());
        this.imageHeadUrl = GlobalConfig.getImgBaseUrl();
        // this.getNotifyList();
        // this.getNewsList();
        // this.getActivityList();
        this.token = LocalStorageUtil.getUserToken();
        this.checkBrowserVersion();
    }

    public checkBrowserVersion() {
        let browserInfo = new TdappModel();
        if (browserInfo.browser_version == 9) {
            this.browserTipShow = true;
        }
    }

    public closeBrowserTip() {
        this.browserTipShow = false;
    }

    /**
     * 设置根路径
     * @param url
     */
    public setBaseUrl(url: string): void {
        this.http.setBaseUrl(url);
    }

    /**
     * 获取下载url
     * @param url
     */
    public onDownloadConfig(jsonConfig: any) {
        const downConfig = jsonConfig.bohe.down_platform[this.webParam.from];
        this.windowsDownloadUrl = downConfig.windows.download_url;
        this.windowsDownloadUrl_old = downConfig.windows.download_url_old;
        $('#bohe_down_old').attr('href',this.windowsDownloadUrl_old)
        this.macDownloadUrl = downConfig.mac.download_url;
    }

    /**
     * 下载windows客户端
     * @param ln
     */
    public windowsDownload() {
        window.location.href = this.windowsDownloadUrl;
    }
    /**
     * 下载windows客户端
     * @param ln
     */
    public windowsDownloadOld() {
        window.location.href = this.windowsDownloadUrl_old;
    }
    /**
     * 切换语言
     */
    public onChangeLanguage(ln: string) {
        lang.changeLanguage(ln);
        i18n.locale = lang.locale;
        this.webParam.language = ln;
        GlobalConfig.log('切换语言:' + lang.locale);
    }

    /**
     * 改变选中按钮
     */
    public changeCheckBtn(index: number) {
        this.checkBtn = index;
    }

    /**
     * 跳转公告详情
     */
    public goNotifyDetail(id: number) {
        JumpWebUtil.gotoNoticeDetails(id);
    }

    /**
     * 跳转公告
     */
    public goNotify() {
        JumpWebUtil.backNotice();
    }

    /**
     * 跳转资讯详情
     */
    public goNewsDetail(id: number) {
        JumpWebUtil.gotoNewsDetails(id);
    }

    /**
     * 跳转资讯
     */
    public goNews() {
        JumpWebUtil.backNews();
    }

    /**
     * 跳转关于薄荷
     */
    public goAbout() {
        JumpWebUtil.backAbout();
    }

    /**
     * 跳转注册
     */
    public goRegister() {
        JumpWebUtil.backRegister();
    }

    /**
     * 获取公告列表
     */
    public async getNotifyList() {
        let url = HttpClient.URL_NEWS;
        let param = new NewRequestModel();
        param.page = 0;
        param.size = 8;
        param.support_type = 1;
        param.class_type = 0;
        param.region_code = LocalStorageUtil.getRegionCodes();
        this.backData = await this.http.get<NewModel>(url, param);
        if (this.backData.code == HttpClient.HTTP_SUCCESS_NET_CODE) {
            this.notifyList = this.backData.data.list;
        }
    }

    /**
     * 获取资讯列表
     */
    public async getNewsList() {
        let url = HttpClient.URL_NEWS;
        let param = new NewRequestModel();
        param.page = 0;
        param.size = 8;
        param.support_type = 1;
        param.class_type = 2;
        param.region_code = LocalStorageUtil.getRegionCodes();
        this.backData = await this.http.get<NewModel>(url, param);
        if (this.backData.code == HttpClient.HTTP_SUCCESS_NET_CODE) {
            this.newsList = this.backData.data.list;
        }
    }

    /**
     * 获取活动列表
     */
    public async getActivityList() {
        let url = HttpClient.URL_ACTIVITY_LIST;
        let param = new ActivityRequestModel();
        param.page = 0;
        param.size = 5;
        param.type = 1;
        param.plat_type = 1;
        param.region_code = LocalStorageUtil.getRegionCodes();
        this.backData = await this.http.get<ActivityRequestModel>(url, param);
        if (this.backData.code == HttpClient.HTTP_SUCCESS_NET_CODE) {
            this.activityList = this.backData.data.list.slice(0, 1);
        }
    }

    /**
     * 打印环境信息
     */
    public debugInfo() {
        let txt = '';
        txt += 'bohe' + '##';
        txt += '当前环境:' + ProjectConfig.server_type + '##';
        txt += '服务器地址:' + GlobalConfig.getBaseUrl() + '##';
        txt += '图片地址:' + GlobalConfig.getImgBaseUrl() + '##';
        txt += '网站地址' + GlobalConfig.getWebBaseUrl();
        ProjectConfig.log(txt);
    }
}

new Index({
    i18n
}).$mount('#app');

```