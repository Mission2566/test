{extend name="../../admin/view/main"}

{block name="content"}
<div class="think-box-shadow" id="TruckForm">
    <div class="layui-card border-line">
        <div class="layui-card-header layui-bg-gray border-bottom-line">
            <b class="color-green">全国</b><span class="font-s12 color-desc margin-left-10">配送省份</span>
            <a class="pull-right notselect" ng-click="SetAllChecked()">全选</a>
        </div>
        <div class="layui-card-body">
            <div class="layui-btn layui-btn-radius margin-left-0 margin-right-5 margin-bottom-5" ng-class="{true:'layui-btn-normal',false:'layui-btn-warm'}[x.status]" ng-click="SetActiveProvince(x)" ng-repeat="x in items">
                <label class="think-checkbox margin-right-0"><input ng-change="SetChangeCity(x,x.status)" ng-model="x.status" type="checkbox" lay-ignore></label><span ng-bind="x.name"></span>
            </div>
        </div>
    </div>

    <div class="layui-card border-line">
        <div class="layui-card-header layui-bg-gray border-bottom-line"><b class="color-green" ng-bind="province.name"></b><span class="font-s12 color-desc margin-left-10">配送城市</span></div>
        <div class="layui-card-body">
            <div class="layui-btn layui-btn-radius margin-left-0 margin-right-5 margin-bottom-5" ng-class="{true:'layui-btn-normal',false:'layui-btn-warm'}[x.status]" ng-click="SetActiveCity(x)" ng-repeat="x in province.subs">
                <label class="think-checkbox margin-right-0"><input ng-change="SetChangeCity(x,x.status)" ng-model="x.status" type="checkbox" lay-ignore></label><span ng-bind="x.name"></span>
            </div>
        </div>
    </div>

    <div class="layui-card border-line">
        <div class="layui-card-header layui-bg-gray border-bottom-line"><b class="color-green" ng-bind="city.name"></b><span class="font-s12 color-desc margin-left-10">配送区域</span></div>
        <div class="layui-card-body">
            <div class="layui-btn layui-btn-radius margin-left-0 margin-right-5 margin-bottom-5" ng-class="{true:'layui-btn-normal',false:'layui-btn-warm'}[x.status]" ng-repeat="x in city.subs">
                <label class="think-checkbox margin-right-0"><input ng-model="x.status" type="checkbox" lay-ignore></label><span ng-bind="x.name"></span>
            </div>
        </div>
    </div>

    <div class="hr-line-dashed margin-top-40"></div>
    <div class="layui-form-item text-center">
        <button class="layui-btn layui-btn-danger" type="button" data-history-back>取消修改</button>
        <button class="layui-btn" ng-click="Confirm()">确定修改</button>
    </div>
</div>

<label class="layui-hide">
    <textarea class="layui-textarea" id="RegionData">{$citys|json_encode|raw}</textarea>
</label>

<script>
    require(['angular'], function () {
        var app = angular.module("TruckForm", []).run(callback);
        var data = document.getElementById('RegionData').value || '[]';
        angular.bootstrap(document.getElementById(app.name), [app.name]);

        function callback($rootScope) {
            $rootScope.items = angular.fromJson(data);
            $rootScope.province = $rootScope.items[0] || {subs: []};
            $rootScope.city = $rootScope.province.subs[0] || [];

            /*! 数据显示状态转换 */
            $rootScope.items.forEach(function (province) {
                province.status = !!province.status;
                if (province.subs) province.subs.forEach(function (city) {
                    city.status = !!city.status;
                    if (city.subs) city.subs.forEach(function (area) {
                        area.status = !!area.status;
