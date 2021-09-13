<template>
    <Page>
        <ActionBar>
            <Label text="Home" />
        </ActionBar>

        <GridLayout rows="auto, *, *">
            <Label class="info">
                <FormattedString>
                    <Span class="fas" text.decode="&#xf135; " />
                    <Span :text="message" />
                </FormattedString>
            </Label>

            <ListView for="item in items" row="1">
                <v-template>
                    <Label :text="item.text" />
                </v-template>
            </ListView>

            <RadListView
                scrollDirection="vertical"
                pullToRefresh="true"
                loadOnDemandMode="Auto"
                loadOnDemandBufferSize="10"
                @pullToRefreshInitiated="onRefresh"
                @loadMoreDataRequested="onLoadMoreItems"
                @itemTap="onItemTap"
                for="item in items"
                row="2"
            >
                <v-template>
                    <Label :text="item.text" />
                </v-template>
            </RadListView>
        </GridLayout>
    </Page>
</template>

<script>
const items = [];

let i = 1;
for (; i <= 100; i++) {
    items.push({
        text: `Item #${i}`
    });
}
export default {
    data() {
        return {
            items
        };
    },
    computed: {
        message() {
            return "Blank {N}-Vue app";
        }
    },
    methods: {
        onRefresh(args) {
            setTimeout(() => {
                this.items.unshift({
                    text: `Prepended item #${i++}`
                })
                args.object.notifyPullToRefreshFinished()
            }, 1000)
        },
        onLoadMoreItems(args) {
            setTimeout(() => {
                for (let j = 0; j < 10; j++) {
                    items.push({
                        text: `Appended item #${i++}`
                    });
                }
                args.object.notifyLoadOnDemandFinished()
            }, 2000)
        },
        onItemTap(args) {
            console.log('tapped', args.item)
        }
    }
};
</script>

<style scoped lang="scss">
// Custom styles
.fas {
    color: #65adf1;
}

.info {
    font-size: 20;
    horizontal-align: center;
    vertical-align: center;
}
</style>