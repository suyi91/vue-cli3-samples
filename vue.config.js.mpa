const HtmlWebpackPlugin = require('html-webpack-plugin');

const PAGE_ENTRIES = [
  'page1',
  'page2',
];

module.exports = {
  devServer: {
    disableHostCheck: true,
    overlay: {
      warnings: false,
      errors: true,
    },
    historyApiFallback: {
      rewrites: PAGE_ENTRIES.map(v => ({
        from: new RegExp(`^\\/${v}`),
        to: `/${v}.html`,
      })),
    },
  },
  chainWebpack: config => {
    config.entryPoints.delete('app')
    config.plugins.delete('html')
    PAGE_ENTRIES.forEach(v => {
      config.entry(v).add(`./src/entries/${v}/main.js`)
      config.plugin(`html-${v}`).use(HtmlWebpackPlugin, [{
        templateParameters: {
          BASE_URL: '/',
        },
        template: `./src/entries/${v}/index.html`,
        entry: `./src/entries/${v}/main.js`,
        filename: `${v}.html`,
        inject: true,
        excludeChunks: PAGE_ENTRIES.filter(item => item !== v),
      }]).before('preload')
    })
  },
  configureWebpack: {
    optimization: {
      splitChunks: {
        cacheGroups: {
          theme: {
            name: 'chunk-theme',
            test: /[\\/]node_modules[\\/]element-ui[\\/]/,
            chunks: 'all',
            priority: 1,
            reuseExistingChunk: true,
            enforce: true,
          },
        },
      },
    },
  },
};
