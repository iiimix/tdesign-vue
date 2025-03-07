<template>
  <!-- 注意控制父元素宽度 -->
  <div style="width: 100%" class="tdesign-demo-block-column-large tdesign-demo-table-multi-head tdesign-demo__tabler">
    <!-- 按钮操作区域 -->
    <t-space>
      <t-checkbox v-model="bordered">显示表格边框</t-checkbox>
      <!-- 只要有 maxHeight，就有固定表头，无论该值是否存在 -->
      <t-checkbox v-model="fixedHeader">显示固定表头</t-checkbox>
      <!-- 为保证组件收益最大化，当数据量小于 `100` 时，无论虚拟滚动的配置是否存在，组件内部都不会开启虚拟滚动 -->
      <!-- <t-checkbox v-model="virtualScroll">虚拟滚动</t-checkbox> -->
      <t-checkbox v-model="fixedLeftCol">固定左侧列</t-checkbox>
      <t-checkbox v-model="fixedRightCol">固定右侧列</t-checkbox>
      <t-checkbox v-model="headerAffixedTop">表头吸顶</t-checkbox>
    </t-space>

    <!-- tableContentWidth 必须大于表格的外层宽度，否则请设置 width: 100% -->
    <!-- 多级表头中，如果要使用固定列功能，则必须设置 colKey 和 fixed -->
    <!-- :scroll="{ type: 'virtual' }" virtual scroll for a lot of data rendered-->
    <t-table
      row-key="index"
      :data="data"
      :sort.sync="sortInfo"
      :columns="columns"
      :bordered="bordered"
      :max-height="fixedHeader ? 380 : undefined"
      :columnController="{ displayType: 'auto-width' }"
      :filterRow="() => null"
      :headerAffixedTop="headerAffixedTop ? { offsetTop: 87 } : false"
      :scroll="{ type: 'virtual' }"
      drag-sort="col"
      resizable
      lazyLoad
      :table-layout="'fixed'"
      @drag-sort="onDragSort"
      @data-change="onDataChange"
      @filter-change="onFilterChange"
    ></t-table>
  </div>
</template>
<script lang="jsx">
import { ErrorCircleFilledIcon, CheckCircleFilledIcon, CloseCircleFilledIcon } from 'tdesign-icons-vue';

const data = [];
for (let i = 0; i < 1000; i++) {
  data.push({
    index: i + 1,
    applicant: ['贾明', '张三', '王芳'][i % 3],
    status: i % 3,
    channel: ['电子签署', '纸质签署', '纸质签署'][i % 3],
    time: [3, 2, 4, 1][i % 4],
    createTime: ['2022-01-01', '2022-02-01', '2022-03-01', '2022-04-01', '2022-05-01'][i % 4],
    property: ['组长审批', '部门审批', '财务审批'][i % 3],
    default: i,
    detail: {
      email: ['w.cezkdudy@lhll.au', 'r.nmgw@peurezgn.sl', 'p.cumx@rampblpa.ru'][i % 3],
      position: `读取 ${i} 个数据的嵌套信息值`,
    },
    needed: i % 4 === 0 ? '是' : '否',
    type_default: '-',
    description: '数据源',
    field1: [100, 200, 400, 500][i % 4],
    field2: [100, 200, 400, 500][i % 4],
    field3: [100, 200, 400, 500][i % 4],
    field4: [100, 200, 400, 500][i % 4],
    field5: '字段5',
    field6: '字段6',
    field7: `审批单号00${i + 1}`,
  });
}

function getColumns(fixedLeftCol, fixedRightCol) {
  return [
    {
      title: '申请人',
      colKey: 'applicant',
      fixed: fixedLeftCol && 'left',
      width: 100,
    },
    {
      title: '申请汇总',
      fixed: fixedLeftCol && 'left',
      width: 100,
      colKey: 'total_info',
      children: [
        {
          align: 'left',
          colKey: 'platform',
          title: '申请状态',
          fixed: fixedLeftCol && 'left',
          width: 120,
          sorter: (a, b) => a.default - b.default,
          cell: (h, { row }) => {
            const statusNameListMap = {
              0: { label: '审批通过', theme: 'success', icon: <CheckCircleFilledIcon /> },
              1: { label: '审批失败', theme: 'danger', icon: <CloseCircleFilledIcon /> },
              2: { label: '审批过期', theme: 'warning', icon: <ErrorCircleFilledIcon /> },
            };
            return (
              <t-tag shape="round" theme={statusNameListMap[row.status].theme} variant="light-outline">
                {statusNameListMap[row.status].icon}
                {statusNameListMap[row.status].label}
              </t-tag>
            );
          },
        },
        {
          title: '申请渠道和金额',
          colKey: 'type_default',
          fixed: fixedLeftCol && 'left',
          width: 100,
          children: [
            {
              align: 'left',
              colKey: 'channel',
              title: '类型',
              fixed: fixedLeftCol && 'left',
              width: 110,
            },
            {
              align: 'center',
              colKey: 'time',
              title: '申请耗时(天)',
              fixed: fixedLeftCol && 'left',
              width: 150,
            },
          ],
        },
      ],
    },
    {
      colKey: 'field1',
      title: '住宿费',
      width: 100,
    },
    {
      colKey: 'field3',
      title: '交通费',
      width: 100,
    },
    {
      colKey: 'field4',
      title: '物料费',
      width: 100,
    },
    {
      colKey: 'field2',
      title: '奖品激励费',
      width: 120,
    },
    {
      title: '审批汇总',
      colKey: 'instruction',
      fixed: fixedRightCol && 'right',
      width: 100,
      children: [
        {
          align: 'left',
          colKey: 'property',
          title: '审批状态',
          fixed: fixedRightCol && 'right',
          width: 120,
          filter: {
            type: 'single',
            list: [
              { label: '所有状态', value: '' },
              { label: '组长审批', value: '组长审批' },
              { label: '部门审批', value: '部门审批' },
              { label: '财务审批', value: '财务审批' },
            ],
          },
        },
        {
          align: 'left',
          ellipsis: true,
          colKey: 'description',
          title: '说明',
          fixed: fixedRightCol && 'right',
          width: 100,
          children: [
            {
              colKey: 'field7',
              title: '审批单号',
              fixed: fixedRightCol && 'right',
              width: 120,
            },
            {
              colKey: 'detail.email',
              title: '邮箱地址',
              fixed: fixedRightCol && 'right',
              ellipsis: true,
              width: 150,
            },
          ],
        },
      ],
    },
    {
      colKey: 'createTime',
      title: '申请时间',
      fixed: fixedRightCol && 'right',
      width: '120',
    },
  ];
}

export default {
  data() {
    return {
      sortInfo: {},
      bordered: true,
      fixedHeader: true,
      fixedLeftCol: false,
      fixedRightCol: false,
      headerAffixedTop: false,
      virtualScroll: true,
      data,
    };
  },
  computed: {
    columns() {
      return getColumns(this.fixedLeftCol, this.fixedRightCol);
    },
  },
  methods: {
    onDataChange(val) {
      this.data = val.concat();
    },
    onFilterChange(filterValue) {
      this.data = data.filter((t) => !filterValue.property || filterValue.property === t.property);
    },
    onDragSort(params) {
      console.log('拖拽排序事件参数：', params);
    },
  },
};
</script>
